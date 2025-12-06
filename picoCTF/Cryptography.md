# Cryptography

# 1. rsa_oracle
Can you abuse the oracle?
An attacker was able to intercept communications between a bank and a fintech company. They managed to get the message (ciphertext) and the password that was used to encrypt the message.
After some intensive reconassainance they found out that the bank has an oracle that was used to encrypt the password and can be found here nc titan.picoctf.net 51826. Decrypt the password and use it to decrypt the message. The oracle can decrypt anything except the password.

## Solution:
Took a lot of time with this one as my windows was double pasting the key. my logic was all correct but i wasnt getting the flag due to this bug. After trying it on my friends laptop, it worked and i realised my windows was the problem

Started with opening the oracle ```nc titan.picoctf.net 60656 ``` , saw it could encrypt or decrypt things, except the password itself
used ``` strings password.enc ``` to get the ciphered password. 
although you cant directly enter the password, I can find x^e mod n (which can be found by encrypting through the nc), then multiplying the two numbers gives (x*m)^e mod n which can then be decrypted and divided by x to find m.
used this property, chosing my number x as 2. although i wanted to use 20, as inputing this would be easier, but computing 20^n would not be that easy . hence i chose a character whose hex would be 2. as this couldnt directly be inputted into the terminal, i had to use ```{ printf 'e\n'; sleep 3; echo -e '\x02\n'; } | nc titan.picoctf.net 60656 ```
computed the math and got the value of m. 
now i had to find the password. here is my calculation 
``` 
>>> a = 1634668422544022562287275254811184478161245548888973650857381112077711852144181630709254123963471597994127621183174673720047559236204808750789430675058597
>>> b = 5067313465613043651275429665315895824157755779222372979446076012356324498190828210335763979330272318657269048435311897896433721115606764442199497891219230
>>> a*b
8283377309369758183523145573200750782484310290735315558059488823972896636253855296781020106079552858693546617489082932997730796347166223270632814660216969565981265960585467439881131219657789014058412904318589178439314193411103546217783791481965173915838239819656233269969451945419284153208337289812023220310
>>> int('68726a6aca',16)
448596175562
>>> 448596175562//2
224298087781
>>> hex(224298087781)
'0x3439353565'
>>> bytes.fromhex('3439353565')
b'4955e'
```
 the password was 4955e

 the hint was openssl. made it easier 

 ```
 openssl enc -aes-256-cbc -d -in secret.enc
enter AES-256-CBC decryption password:
*** WARNING : deprecated key derivation used.
Using -iter or -pbkdf2 would be better.
picoCTF{su((3ss_(r@ck1ng_r3@_4955eb5d}

```




## Flag: 
picoCTF{su((3ss_(r@ck1ng_r3@_4955eb5d}
## Concepts Learnt: 

## Notes:

## Resources: 

# 2. Custom encryption
Can you get sense of this code file and write the function that will decode the given encrypted file content.
Find the encrypted file here flag_info and code file might be good to analyze and get the flag.


## Solution:
download gave me two files, a python script and a file with no extension
running ``` strings enc_flag ``` gave me 
``` 
a = 90
b = 26
cipher is: [61578, 109472, 437888, 6842, 0, 20526, 129998, 526834, 478940, 287364, 0, 567886, 143682, 34210, 465256, 0, 150524, 588412, 6842, 424204, 164208, 184734, 41052, 41052, 116314, 41052, 177892, 348942, 218944, 335258, 177892, 47894, 82104, 116314]

```

next i read the python script and figured out what it did. 

the python script had to be reverse engineered, after figuring how the encryption process worked. to understand the py script, i commented directly on the py script and my working is shown below: 
```
from random import randint
import sys


def generator(g, x, p): #a=90, b=26, g=31,p=97, gap
    return pow(g, x) % p


def encrypt(plaintext, key):
    cipher = []
    for char in plaintext:
        cipher.append(((ord(char) * key*311)))
    return cipher


def is_prime(p):
    v = 0
    for i in range(2, p + 1):
        if p % i == 0:
            v = v + 1
    if v > 1:
        return False
    else:
        return True


def dynamic_xor_encrypt(plaintext, text_key): #passed: plain text , key = trudeau
    cipher_text = ""
    key_length = len(text_key) #key_lenght = 7
    for i, char in enumerate(plaintext[::-1]): #input reversed, sid = dis, d 0, i 1, s 2
        key_char = text_key[i % key_length]
        encrypted_char = chr(ord(char) ^ ord(key_char))
        
        cipher_text += encrypted_char
    return cipher_text


def test(plain_text, text_key):
    p = 97
    g = 31
    if not is_prime(p) and not is_prime(g):
        print("Enter prime numbers")
        return
    a = randint(p-10, p) #a=90, b=26, g=31,p=97, gap, 
    b = randint(g-10, g)
    print(f"a = {a}")
    print(f"b = {b}")
    u = generator(g, a, p) #u=64
    v = generator(g, b, p)
    key = generator(v, a, p)
    b_key = generator(u, b, p) # b_key=22
    shared_key = None
    if key == b_key:
        shared_key = key #shared key =22, 
    else:
        print("Invalid key")
        return
    semi_cipher = dynamic_xor_encrypt(plain_text, text_key) #plain text = text, key = trudeau
    cipher = encrypt(semi_cipher, shared_key)
    print(f'cipher is: {cipher}')


if __name__ == "__main__":
    message = sys.argv[1]
    test(message, "trudeau")
#passes string  entered to test along with trudeau, message is plain text, text key is treaudeu

```

i now had to make a looping py script, which reversed the operation in order. here is my py script:
```
cipher = [61578, 109472, 437888, 6842, 0, 20526, 129998, 526834, 478940, 287364, 0, 567886, 143682, 34210, 465256, 0, 150524, 588412, 6842, 424204, 164208, 184734, 41052, 41052, 116314, 41052, 177892, 348942, 218944, 335258, 177892, 47894, 82104, 116314]

p = 97
g = 31
a = 90
b = 26

def generator(g, x, p):
    return pow(g, x) % p

v = generator(g, b, p)
key = generator(v, a, p)

semi_cipher = []
flag = ""

for i in cipher:
    char = i//(311*key)
    semi_cipher.append(chr(char))
    
for i, char in enumerate(semi_cipher):
    key_char = "trudeau"[i % 7]
    decrypted_char = chr(ord(char) ^ ord(key_char))
    flag += decrypted_char

print(flag[::-1])

```
## Flag: 
picoCTF{custom_d2cr0pt6d_49fbee5b}

## Concepts Learnt: 


## Notes:

## Resources: 

# 3. MiniRSA
Let's decrypt this: ciphertext? Something seems a bit small. 

## Solution:
saw the wiki page and also asked gpt on how RSA works. as the power was quite less, it was easy to take a cube root of such a number. wrote a quick python script to find the cube root of the number(using binary search), converted this hex to text, which revealed the flag


## Flag: 
 picoCTF{n33d_a_lArg3r_e_606ce004}

## Concepts Learnt: 

## Notes:

## Resources: 