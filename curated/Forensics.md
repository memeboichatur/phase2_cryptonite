# 1. Hide and Seek
Sakamoto’s at it again with a game of hide and seek, but this time, it’s not with Shin or his daughter. An old friend hid some secret data in this image. Can you find it before the others do?
Hint:
Even in retirement, Sakamoto never loses at hide and seek. Maybe stegseek can help you keep up.

## Solution:
i first ran steghide to figure out if the flag was there. i realised a password was needed. some looking around, i realized i could use rockyou, a list of common passwords. using this, i ran stegseek. it gave me a file in a .out file. i converted this to a .txt file and the flag popped right up

```
memeboichatur@xenon:/mnt/c/Users/chatu/OneDrive/Desktop/jtp2/challenge_files/forensics/hidenseek$ stegseek sakamoto.jpg rockyou.txt
StegSeek 0.6 - https://github.com/RickdeJager/StegSeek

[i] Found passphrase: "iloveyou1"
[i] Original filename: "flag.txt".
[i] Extracting to "sakamoto.jpg.out".

memeboichatur@xenon:/mnt/c/Users/chatu/OneDrive/Desktop/jtp2/challenge_files/forensics/hidenseek$ file sakamoto.jpg.out
sakamoto.jpg.out: ASCII text
memeboichatur@xenon:/mnt/c/Users/chatu/OneDrive/Desktop/jtp2/challenge_files/forensics/hidenseek$ mv sakamoto.jpg.out sakamoto.txt

```

## Flag: 
nite{h1d3_4nd_s33k_but_w1th_st3g_sdfu9s8}

## Concepts Learnt: 
using steghide and rockyou
## Notes:

## Resources: 

x-------------------------------------------------------------------x

# 2. Nutrela Chunks
One of my favorite foods is soya chunks. But as I was enjoying some Nutrela today, I noticed a few chunks weren’t quite right. Seems like something’s off with their structure. Could you help me fix these broken chunks so I can enjoy my meal again?

## Solution:
opening the file didnt work. the description talked about broken chunks. i had solved a similar problem before, where the magic bytes were messy. i tried using the same approach here. i compared the hex of this code with that of a standard one, and realized capitalization was the issue. fixing that, i got the image to open revealing the flag.


## Flag: 
nite{n0w_y0u_kn0w_ab0ut_PNG_chunk5}

## Concepts Learnt: 
magic numbers, hex and how they work

## Notes:

## Resources: 
https://medium.com/@0xwan/png-structure-for-beginner-8363ce2a9f73

x-----------------------------------------------------------------------------x

# 3. RAR Of The Abyss
Two philosophers peer into the networked abyss and swap a secret. Use the secret to decrypt the Abyss’ RAwR and pull your flag from the void.

## Solution:
i opened the file in wireshark. went to the conversations tab.
```
Camus: One must imagine Sisyphus happy but are we happy ?
Nietzsche: You will be happy after reading my latest work
Camus: whats the password ?
Nietzsche: b3y0ndG00dand3vil
Camus: thanks
```

revealed a password for something more...

i also found a file that matched a .rar file. downloading ```output.bin``` and extracting using the password above
i got the flag.

## Flag: 
nite{thus_sp0k3_th3_n3tw0rk_f0r3ns1cs_4n4lyst}

## Concepts Learnt: 
rar file headers
wireshark
## Notes:

## Resources: 


x---------------------------------------------------------------------------------x

# 4. NineTails
Looks like I got a little too clever and hid the flag as a password in Firefox, tucked away like one of NineTails’ many tails. Recover the "logins" and the "key4" and let it guide you to the flag. Hint: I named my Ninetails "j4gjesg4", quite a peculiar name isn't it?

## Solution:
extracting the given file, i got an ad1 file. opened the file in ftk-imager. looking around the file tree, i found the file ``` logins.json``` and a key.

installing the firefox decrypt package from gitthub and running it revealed the flag

## Flag: 
GCTF{m0zarella_f1ref0x_p4ssw0rd}

## Concepts Learnt: 
ftk imager
ad1 files
## Notes:

## Resources: 

x--------------------------------------------------------------------------------x

# 5. Re:Draw
Her screen went black and a strange command window flickered to life, lines of text flashed across before everything went silent. Moments later, the system crashed. By sheer luck, we recovered a memory dump. Note: There are three stages to this challenge and you will find three flags. What we know: just before the crash, a black command window flickered across the screen, something in its output might still be visible if you dig through memory. She was drawing when it happened, and remnants of a painting program linger, which could reveal more if inspected in the right way. Finally, a mysterious archive hides deeper in memory, likely holding the last piece of her work. 

Hint: Learn up on volatility 2 and its various plugins and what they are used for.


## Solution:
this challenge was really lenghty. i started with extracting the zip file and got a .raw file. ran a kdbgscan on the .raw file revealed it was a win7sp1 file

next i used pslist, showing 3 main processes-> winrar, mspaint and cmd

ran consoles on the cmd revealed a encrypted string (base64) and got the first flag

used memdump on the winrar section gave a .dmp file
to find the offset in this file, checked the hex
``` FF FF FF FF ``` revelaed it was a RGBA image. adjusting the height filter, i found inverted text giving the second flag 
(i am good boy)


next i found a .rar file being given as an argument to winrar
i needed a NTLM hash of the password. using the hashdump command i got a png file

the png file gave me the third flag

## Flag: 
flag{th1s_1s_th3_1st_st4g3!!}
flag{Good_Boy_good_girl}
flag{w3ll_3rd_stage_was_easy}

## Concepts Learnt: 

## Notes:

## Resources: 
https://grokipedia.com/page/Pwdump
https://github.com/volatilityfoundation/volatility