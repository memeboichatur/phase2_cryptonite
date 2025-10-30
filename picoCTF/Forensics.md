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
as the file format was unknown, i used ``` file tunn3l_v1s10n ``` to figure out what kind of a file it was. this gave me info that the file is a data file. as this was not enough, i decided to look at the hex of the picture, as this would bring some more clarity. the magic numbers ```42 4D``` revealed it was a .bmp image. renaming it directly to bmp didnt work, which i hoped it would. instead it gave me a header error. after a lot of googling on how headers work, and trying to compare my files values to other normal bmp hex codes, i realised the corrupted part was BA D0. Asking gpt told me i should rename this to 0x28, which meant 40(this was probably a standard of bmp files). The file finally opened !!. However it said not a flag :( . as i now had a working bmp file, i decided to check its metadata, finding nothing. i tried steganography, with many combinations of the not flag as password to no avail. opening the file up, i did see the file size to be 2.8 mb , which was quite large for a file of dimensions 1134x306.maybe the file was not displaying fully (tunnel vision?). this was a good hint. checking the height column,it wsa restricted to read only a part of the file, editing this to a larger value gave me the flag.


## Flag: 
picoCTF{qu1t3_a_v13w_2020}

## Concepts Learnt: 
bmp headers and basics of how they work.


## Notes:

## Resources: 
https://hexed.it/
steganography tools.

# 3.m00nwalk
Decode this message from the moon


## Solution:
downloading the file i had no clue what kind of a message this could be. looking at the first hint: ``` How did pictures from the moon landing get sent back to Earth? ``` . googled this to find out they used radio to tx and rx images. this gave a good idea on where to start- sstv. opened up a sstv decoder online and it instantly gave me the flag containing the image.

## Flag: 
picoCTF{beep_boop_im_in_space}

## Concepts Learnt: 
basics of sstv 

## Notes:

## Resources: 
 https://sstv-decoder.mathieurenaud.fr/
 google