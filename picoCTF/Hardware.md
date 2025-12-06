# Hardware

# 1. IQ Test
let your input x = 30478191278. wrap your answer with nite{ } for the flag. As an example, entering x = 34359738368 gives (y0, ..., y11), so the flag would be nite{010000000011}.
## Solution:
converted the given number to binary using gemini. the binary converted was of 35 digits only but the question asked for 36 inputs, so I added a 0 at the most significant bit. now it was just manual tracing of the circuit and assembling the flag


## Flag: 
nite{100010011000}

## Concepts Learnt: 
logic gates
how tuff pre gpt manual days truly were

## Notes:

## Resources: 

# 2. I Like Logic
i like logic and i like files, apparently, they have something in common, what should my next step be?

## Solution:
checked strings on each file. file 3 had some aditional stuff into it. it said <SALEAE>. googled this up and found their site. ``` 
Effortlessly decode protocols like SPI, I2C, Serial, and many more. Leverage community created analyzers or build your own low-level or high-level protocol analyzer. ```

gave me a good hint it could be related to finding the flag as it was related to hardware. this put me on the right tangent. installed 'Logic 2' and loaded the .sal file on it. loaded the analysers and selected input channel 3, and i got sum story in the data box. read the interesting story and found the flag hidden inside between it :)
the flag also didnt look like much of a flag, googled FCSC and found its the french cybersec challenge, so this must be it.

## Flag: 
FCSC{b1dee4eeadf6c4e60aeb142b0b486344e64b12b40d1046de95c89ba5e23a9925}

## Concepts Learnt: 

## Notes:

## Resources: 

# 3. Bare Metal Alchemist
my friend recommended me this anime but i think i've heard a wrong name.

## Solution:
ran the strings command, file command etc to understand what the .elf file was. then tried using ghidara to decompile the file. opened up the main function.
i was seeing the ```z1()``` a lot of time, so i tried to figure out what it did. it was overwriting certain data
more looking around, i found that the code was reading bytes from 0x68 and then XORing it with 0xA5, in a loop. the loop exited if the current data was 0xa5 or the key.
i went to 0x0068 and found hex there 

next, i xored each byte with 0XA5 to get 
```54 46 43 43 54 46 7b 54 68 31 73 5f 31 73 5f 73 6f 6d 33 5f 73 31 6d 70 6c 33 5f 34 72 64 75 31 6e 6f 5f 66 31 72 6d 77 34 72 65 7d ``` . converted this to ASCII to get the flag. it was indeed not simple arduino firmware :(

## Flag: 
TFCCTF{Th1s_1s_som3_s1mpl3_4rdu1no_f1rmw4re}

## Concepts Learnt: 
how 2 use ghidara
how 2 read ghidara

## Notes:

## Resources: 
ghidara