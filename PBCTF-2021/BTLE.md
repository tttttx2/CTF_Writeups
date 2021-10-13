# Description
We are presented with a pcap file containing Bluetooth LE packages. We can see that we have many writes to memory with a specific value and offset.

# Plan
Extract the data, build a fake memory in an array and write the hex values there. Then see if we can read this

# Solution
Extract the values from the pcap using the following tshark command
```bash
tshark -r btle.pcap -Y "btatt.opcode == 0x16" -T fields -e btatt.value -e btatt.offset > export
```

Then parse the file and add everything to the memory. Keep in mind that characters are hex encoded
```python
mem = [" " for i in range(0,150)]

def write(value, offset):
    count = 0
    value = [value[i:i+2] for i in range(0, len(value), 2)]
    for i in value:
        mem[count+offset]=chr(int(i,base=16))
        count += 1
        
with open("export") as file:
    while (line := file.readline().rstrip()):
        line = line.split(',')
        write(line[0],int(line[1]))

print(''.join(mem))
```

This prints the flag ``` pctf{b1Ue_te3Th_is_ba4d_4_y0u_jUs7_4sk_HAr0lD_b1U3tO07h}```, however since the flag format is pbctf we need to correct it as the tshark filter missed the first direct write of 'pb'
