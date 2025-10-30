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


## Flag: 

## Concepts Learnt: 

## Notes:

## Resources: 