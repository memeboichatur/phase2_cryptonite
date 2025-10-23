# Reverse Engineering

# 1. GDB baby step 1 
Can you figure out what is in the eax register at the end of the main function? Put your answer in the picoCTF flag format: picoCTF{n} where n is the contents of the eax register in the decimal number base. If the answer was 0x11 your flag would be picoCTF{17}.

## Solution: 
downloaded the file. used gdb to disassemble the machine code. then read this code to figure out line <15> noved the eax value. took this hex code and converted it to decimal(using gdb). formed the flag.

reference code:

```
memeboichatur@xenon:/mnt/c/Users/chatu/OneDrive/Desktop/jtp 2/gdb_baby_1$ chmod +x debugger0_a
memeboichatur@xenon:/mnt/c/Users/chatu/OneDrive/Desktop/jtp 2/gdb_baby_1$ gdb debugger0_a
GNU gdb (Ubuntu 12.1-0ubuntu1~22.04.2) 12.1
Copyright (C) 2022 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
Type "show copying" and "show warranty" for details.
This GDB was configured as "x86_64-linux-gnu".
Type "show configuration" for configuration details.
For bug reporting instructions, please see:
<https://www.gnu.org/software/gdb/bugs/>.
Find the GDB manual and other documentation resources online at:
    <http://www.gnu.org/software/gdb/documentation/>.

For help, type "help".
Type "apropos word" to search for commands related to "word"...
Reading symbols from debugger0_a...
(No debugging symbols found in debugger0_a)
(gdb) set disassembly-flavour intel
No symbol table is loaded.  Use the "file" command.
(gdb) disassemble main
Dump of assembler code for function main:
   0x0000000000001129 <+0>:     endbr64
   0x000000000000112d <+4>:     push   %rbp
   0x000000000000112e <+5>:     mov    %rsp,%rbp
   0x0000000000001131 <+8>:     mov    %edi,-0x4(%rbp)
   0x0000000000001134 <+11>:    mov    %rsi,-0x10(%rbp)
   0x0000000000001138 <+15>:    mov    $0x86342,%eax
   0x000000000000113d <+20>:    pop    %rbp
   0x000000000000113e <+21>:    ret
End of assembler dump.
(gdb) print/d 0x86342
$1 = 549698
(gdb)
[1]+  Stopped                 gdb debugger0_a
memeboichatur@xenon:/mnt/c/Users/chatu/OneDrive/Desktop/jtp 2/gdb_baby_1$ exit

```

## Flag: 
picoCTF{549698}

## Concepts Learnt: 
disassembling a file and reading its machine code.
  
## Notes:
n/a

## Resources: 
https://gemini.google.com/ - used gemini to learn about rev engg and understand the challenge.




# 2.  ARMssembly 1
For what argument does this program print `win` with variables 83, 0 and 3? File: chall_1.S Flag format: picoCTF{XXXXXXXX} -> (hex, lowercase, no 0x, and 32 bits. ex. 5614267 would be picoCTF{0055aabb})

## Solution:

## Flag: 

## Concepts Learnt: 
difference between .s and .S files
we need to compile these files first, before loading them into gdb

## Notes:

## Resources: 


