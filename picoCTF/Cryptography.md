# Cryptography

# 1. rsa_oracle
Can you abuse the oracle?
An attacker was able to intercept communications between a bank and a fintech company. They managed to get the message (ciphertext) and the password that was used to encrypt the message.
After some intensive reconassainance they found out that the bank has an oracle that was used to encrypt the password and can be found here nc titan.picoctf.net 51826. Decrypt the password and use it to decrypt the message. The oracle can decrypt anything except the password.

## Solution:

## Flag: 

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

``` writeup needs to be completed ```


## Flag: 
picoCTF{custom_d2cr0pt6d_49fbee5b}

## Concepts Learnt: 


## Notes:

## Resources: 