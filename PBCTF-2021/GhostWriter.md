# Description
We get a simple wav file of someone typing a message (275 char long) on a mechanical keyboard. There's approximately 2 characters per second, almost complete silence in between and we know it's only [a-z\s]

# Plan
Split the audio into shorter audio samples for each key, assign a letter to each and solve the resulting substitution cipher

# Solution
In order to split everything up we created a simple python script that recognizes the beginning and end of each keypress by looking for near silence. Then we calculate the average value (simply the "volume" of the keypress, ignore frequency stuff) using the following:

```python
from scipy.io import wavfile
samplerate, data = wavfile.read('./output.wav')

i = 0
countsilent = 0
samplefound = 0

avg = []
start = 0
end = 0
avg.append([0])

for sample in data:
    i+=1	
    if(sample[1]<100):
        countsilent += 1
    if(countsilent > 10000 and sample[1]>100):
        countsilent = 0
        #print(str(i)+": sample found")
        start = i
        samplefound = 1
    if(countsilent > 8000 and samplefound==1):
        samplefound = 0
        #print(str(i)+": sample ended")
        end = i
        avg[len(avg)-1]=avg[len(avg)-1]/(end-start)
        print("avg: "+str(avg[len(avg)-1]))
        avg.append([0])
    if (samplefound == 1):
        avg[len(avg)-1] += sample[1]

```

We now see that we have matching average values for specific keys, if that wouldn't have happened already simpy rounding until it does could help but wasn't necessary. The following script parses those average values and assigns each one to a letter, basically creating a substitution cipher.
```python
alphabet = "abcdefghijklmnopqr stuvwxyz"
avg = {}

i=0

res=""

with open("avg") as file:
    for line in file:
        value = line.rstrip()[6:-1]
        #print(value)
        if str(value) not in avg:
            avg[str(value)]=alphabet[i]
            i+=1
        res+=avg[str(value)]
print(avg)
print(res)
```

Using an online substitution cipher solver (here)[https://www.guballa.de/substitution-solver] gives us the (near perfect) flag.

```
pbctf{mechanical_keyboards_are_loud}
```

