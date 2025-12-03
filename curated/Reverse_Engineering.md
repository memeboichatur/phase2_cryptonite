# 1. JoyDivision

## Solution:
started with ```file disorder``` to get \
```disorder: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=f889e30b5b0d5265c47d47f04584385c9e25fee6, for GNU/Linux 3.2.0, not stripped```
the file was elf. next tried running the file

```
./disorder

May Jupiter strike you down Caeser before you seize the treasury!! You will have to tear me apart
for me to tell you the flag to unlock the Roman Treasury and fund your civil war. I, Lucius Caecilius
Metellus, shall not let you pass until you get this password right. (or threaten to kill me-)

Segmentation fault (core dumped) 
```

text had hints of "tearing it apart". ghidra was my best bet here

opening up the file in ghidra and looking at main, i figured the program took palatinepackflag.txt as an input, performed some operations on it and then stored the output in flag.txt. now i just had to reveng the program to assemble palatinepackflag.txt
the program also called on flipbits and expand. so i read those programs next

the function flipbits modified the buffer.
it performed the NOT(~) operation on even indices and XORed odd indices with a variable key, which started with (0x69)aka(105) and incrimented by (0x20)aka(32)

the function expand expanded the buffer size, and was called 3 times.
it took input buffer and created a new buffer twice the size.
splits every byte into 4 bits.
It hides these into two new bytes, masked with a variable key.
Logic: Top 4 bits put into Byte A, Bottom 4 bits put into Byte B.

using python and gpt, i made a script to reverse the function.

```
def flipBits(data):
    # Re-implements the C logic exactly to reverse the XOR/NOT operations
    chars = list(data)
    key = 0x69
    toggle = False
    for i in range(len(chars)):
        if toggle:
            chars[i] ^= key
            key = (key + 0x20) & 0xFF
        else:
            chars[i] = (~chars[i]) & 0xFF
        toggle = not toggle
    return bytes(chars)

def shrink(data):
    # Reverses the 'expand' function by combining nibbles
    output = []
    toggle = False
    for i in range(0, len(data), 2):
        byte1 = data[i]
        byte2 = data[i+1]
        if not toggle:
            # Even logic: Lower nibble is in byte1, Upper in byte2
            val = (byte2 & 0xF0) | (byte1 & 0x0F)
        else:
            # Odd logic: Upper nibble is in byte1, Lower in byte2
            val = (byte1 & 0xF0) | (byte2 & 0x0F)
        output.append(val)
        toggle = not toggle
    return bytes(output)

# Execution flow
with open("flag.txt", "rb") as f:
    encrypted = f.read()

stage1 = shrink(encrypted) # Reverse 3rd expand
stage2 = shrink(stage1)    # Reverse 2nd expand
stage3 = shrink(stage2)    # Reverse 1st expand
flag = flipBits(stage3)    # Reverse flipBits

print(flag.decode())

```


this printed the flag to the console when run


## Flag: 
sunshine{C3A5ER_CR055ED_TH3_RUB1C0N}

## Concepts Learnt: 
using ghidra. 
learning to reverse a script logic.


## Notes:
pls rename variables or else logic becomes a mess


## Resources: 

