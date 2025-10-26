# Forensics

# 1. Trivial Flag Transfer Protocol
Figure out how they moved the flag.

## Solution:
This one was quite interesting. Had no idea what a .pcapng file is. After some searching figured out what it is, and that i could read what it does with wireshark. Downloaded wireshark and after some looking, got 6 files downloaded. three of them were images, an instruction and plan file, and also a program. opened up the instructions file to figure out it was encoded. asking gpt i got the decoded messages which hinted the flag was somehow hidden in the 3 images. 

Incorrect Tangent: As 3 images were given, I thought the flag must be hidden in a combination of these 3 images. Ran many differential image programs hoping to find a flag, with no success.
At this point I hadnt opened up the .deb program.

installing the program lead me on the right challenge- checking steganography. after realising i would need some sort of a password, some tinkering around made me think the password could be in something that i decoded. After a lot of hit and trial, the third file worked with the password: DUEDILIGENCE

finally, i got the flag :) 

## Flag: 
picoCTF{h1dd3n_1n_pLa1n_51GHT_18375919}

## Concepts Learnt: 
basics of wireshark and tftp(im still not too sure how these work, just opening the program and tinkering gave me the files without understanding much of what was going on)
steganography

## Notes:
n/a

## Resources: 
https://futureboy.us/stegano/decinput.html - steganograpghic tool
wireshark download

# 2.tunn3l v1s10n
We found this file. Recover the flag.

## Solution:

## Flag: 

## Concepts Learnt: 

## Notes:

## Resources: 
 