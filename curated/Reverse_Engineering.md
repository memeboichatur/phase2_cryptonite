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
ghidra


x-------------------------------------------------------------------------------------------------x

# 2. Worthy Knight

## Solution:
looking at the file, it was a .knight file. as this was not any standard file, i used ```file worthy.knight``` to figure out its a elf stripped file. my best bet here was ghidra. i first ran the file and figured the program needed a 10 letter passphrase. loading the file into ghidra, i figured the program was doing a literal character match with half of the characters directly given, and the rest being xored and compared. reverse engineering this was easy and i did this manually to obtain all but the 5th and 6th character of the phrase. the password obtained till now was ```Njk8__YaIi```.

## Flag: 

## Concepts Learnt: 

## Notes:

## Resources: 
ghidra

# 3. time

## Solution:
started with ```file time``` to learn its an unstripped linux ex. ran it and figured out it needed me to guess a random number. it then printed out the random number and gave the correct/incorrect message by comparing the random number. the best method here was to use gdb and set a breakpoint after the number was generated and before it asked for the input.

``` 


(gdb) disas main
Dump of assembler code for function main:
   0x000000000040092b <+0>:     push   %rbp
   0x000000000040092c <+1>:     mov    %rsp,%rbp
   0x000000000040092f <+4>:     sub    $0x20,%rsp
   0x0000000000400933 <+8>:     mov    %edi,-0x14(%rbp)
   0x0000000000400936 <+11>:    mov    %rsi,-0x20(%rbp)
   0x000000000040093a <+15>:    mov    %fs:0x28,%rax
   0x0000000000400943 <+24>:    mov    %rax,-0x8(%rbp)
   0x0000000000400947 <+28>:    xor    %eax,%eax
   0x0000000000400949 <+30>:    mov    $0x0,%edi
   0x000000000040094e <+35>:    call   0x400750 <time@plt>
   0x0000000000400953 <+40>:    mov    %eax,%edi
   0x0000000000400955 <+42>:    call   0x400730 <srand@plt>
   0x000000000040095a <+47>:    call   0x400790 <rand@plt>
   0x000000000040095f <+52>:    mov    %eax,-0xc(%rbp)
   0x0000000000400962 <+55>:    lea    0x1c7(%rip),%rdi        # 0x400b30
   0x0000000000400969 <+62>:    call   0x4006e0 <puts@plt>
   0x000000000040096e <+67>:    lea    0x1e3(%rip),%rdi        # 0x400b58
   0x0000000000400975 <+74>:    call   0x4006e0 <puts@plt>
   0x000000000040097a <+79>:    lea    0x207(%rip),%rdi        # 0x400b88
   0x0000000000400981 <+86>:    call   0x4006e0 <puts@plt>
   0x0000000000400986 <+91>:    lea    0x21b(%rip),%rdi        # 0x400ba8
   0x000000000040098d <+98>:    mov    $0x0,%eax
   0x0000000000400992 <+103>:   call   0x400710 <printf@plt>
   0x0000000000400997 <+108>:   mov    0x2006ea(%rip),%rax        # 0x601088 <stdout@@GLIBC_2.2.5>
   0x000000000040099e <+115>:   mov    %rax,%rdi
   0x00000000004009a1 <+118>:   call   0x400760 <fflush@plt>
   0x00000000004009a6 <+123>:   lea    -0x10(%rbp),%rax
   0x00000000004009aa <+127>:   mov    %rax,%rsi
--Type <RET> for more, q to quit, c to continue without paging--break *0x400992
   0x00000000004009ad <+130>:   lea    0x208(%rip),%rdi        # 0x400bbc
   0x00000000004009b4 <+137>:   mov    $0x0,%eax
   0x00000000004009b9 <+142>:   call   0x400780 <__isoc99_scanf@plt>
   0x00000000004009be <+147>:   mov    -0x10(%rbp),%eax
   0x00000000004009c1 <+150>:   mov    %eax,%esi
   0x00000000004009c3 <+152>:   lea    0x1f5(%rip),%rdi        # 0x400bbf
   0x00000000004009ca <+159>:   mov    $0x0,%eax
   0x00000000004009cf <+164>:   call   0x400710 <printf@plt>
   0x00000000004009d4 <+169>:   mov    -0xc(%rbp),%eax
   0x00000000004009d7 <+172>:   mov    %eax,%esi
   0x00000000004009d9 <+174>:   lea    0x1f3(%rip),%rdi        # 0x400bd3
   0x00000000004009e0 <+181>:   mov    $0x0,%eax
   0x00000000004009e5 <+186>:   call   0x400710 <printf@plt>
   0x00000000004009ea <+191>:   mov    0x200697(%rip),%rax        # 0x601088 <stdout@@GLIBC_2.2.5>
   0x00000000004009f1 <+198>:   mov    %rax,%rdi
   0x00000000004009f4 <+201>:   call   0x400760 <fflush@plt>
   0x00000000004009f9 <+206>:   mov    -0x10(%rbp),%eax
   0x00000000004009fc <+209>:   cmp    %eax,-0xc(%rbp)
   0x00000000004009ff <+212>:   jne    0x400a14 <main+233>
   0x0000000000400a01 <+214>:   lea    0x1e0(%rip),%rdi        # 0x400be8
   0x0000000000400a08 <+221>:   call   0x4006e0 <puts@plt>
   0x0000000000400a0d <+226>:   call   0x400877 <giveFlag>
   0x0000000000400a12 <+231>:   jmp    0x400a20 <main+245>
   0x0000000000400a14 <+233>:   lea    0x1fd(%rip),%rdi        # 0x400c18
   0x0000000000400a1b <+240>:   call   0x4006e0 <puts@plt>
   0x0000000000400a20 <+245>:   mov    0x200661(%rip),%rax        # 0x601088 <stdout@@GLIBC_2.2.5>
   0x0000000000400a27 <+252>:   mov    %rax,%rdi
   0x0000000000400a2a <+255>:   call   0x400760 <fflush@plt>
   0x0000000000400a2f <+260>:   mov    $0x0,%eax
   0x0000000000400a34 <+265>:   mov    -0x8(%rbp),%rdx
   0x0000000000400a38 <+269>:   xor    %fs:0x28,%rdx
   0x0000000000400a41 <+278>:   je     0x400a48 <main+285>
   0x0000000000400a43 <+280>:   call   0x400700 <__stack_chk_fail@plt>
--Type <RET> for more, q to quit, c to continue without paging--c
   0x0000000000400a48 <+285>:   leave
   0x0000000000400a49 <+286>:   ret
End of assembler dump.
(gdb) break *0x400992
Breakpoint 1 at 0x400992
(gdb) run
Starting program: /mnt/c/Users/chatu/OneDrive/Desktop/jtp2/challenge_files/reveng/time/time
[Thread debugging using libthread_db enabled]
Using host libthread_db library "/lib/x86_64-linux-gnu/libthread_db.so.1".
Welcome to the number guessing game!
I'm thinking of a number. Can you guess it?
Guess right and you get a flag!

Breakpoint 1, 0x0000000000400992 in main ()
(gdb) x/d $rbp-0xc
0x7fffffffdfb4: 2088080361
(gdb) c
Continuing.
Enter your number: 2088080361
Your guess was 2088080361.
Looking for 2088080361.
You won. Guess was right! Here's your flag:
Flag file not found!  Contact an H3 admin for assistance.
[Inferior 1 (process 2170) exited normally]
(gdb)




```

i figured when the random number was being stored, and then set a breakpoint and ran the code. 
``` (gdb) break *0x400992 ```
next, i had to print the stored random number. this was doing using
``` (gdb) x/d $rbp-0xc ```

giving me the number 2088080361 
i then inputted this number in the program giving me the success message.

no flag was generated here but the success message was enough to pass.
## Flag: 
n/a
## Concepts Learnt: 

## Notes:

## Resources: 
gdb


x------------------------------------------------------------------------------------------------------x

# 4. Verdis Quo

## Solution:
started with installing the apk and launching the app. i realised it opened and closed instantly, prompting a too slow message. next i opened the file up in jadx to look at the source code. i looked at the main function(entry point function) called mainactivity.java
The program immediately calls ```util.cleanUp()``` before setting a default failure message.
looking at the decompiled code in Utilities.java. The cleanUp() function contains some lines of code all setting parts of the flag to ' '
or basically erasing it. this meant the flag had to be stored somewhere. i next went to the layout file res/layout/activity_main.xml

here i found the flag hardcoded, but looking at the letters, they were jumbled up. i put this in gpt to figure out a possible readable combination, and got the flag.

## Flag: 
byuctf{decryption_is_difficult_4u}

## Concepts Learnt: 
reading jdk and android development.

## Notes:

## Resources: 
jdk
bluestacks


# 5. Dusty

## Solution:
we were given 3 files: dust_noob, dust_intermediate, dust_pro

starting with dust_noob, i ran ``` file dust_noob``` to find its a 64 bit elf, not stripped. i loaded it into ghidra. after an hour of trying to make sense of the decompiled code, i was reaching nowhere. some quick searching around later i realised the program was written using rust- a language notorious for not being able to be decompiled that easily. i was forced to now read assembly to solve the challenge.

using gdb

## Flag: 

## Concepts Learnt: 

## Notes:

## Resources: 