# Description
We get a flac file containing a radio transmission. Get the message (it's not stego!)

# Plan
Find the transmission protocol used and decode it.

# Solution
Listening to the audio file one might recognize it being similar to APRS messages. If you don't, head over to sigint wiki and compare known protocol samples until you find it. I did the same ;)

APRS is using AFSK.1200, so decoding it with multimon-ng results a message. Unfortunately the payload is further encoded. The first part of the message hints to the message standard though and after some research you'll find that it's WinLink!

The easy way out is to simply setup soundmodem and WinLink (follow a manual online step by step for configuration), put in the receiver callsign and gateway callsign from the beginning of the message and find a way to route audio into soundmodem (just redirect speaker output to mic input). If you did it right you'll receive the WinLink Email, containing the base64 encoded flag.

Basically, play around with old ham radio software on windows until you figure out how to get them running. While it was fun, and as a licensed ham radio operator myself I had a bit of an advantage of knowing my way around stuff, setting up the software was slightly annoying.

Flag ```pbctf{90823jsdghk_8013ks7234}```
