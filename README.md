# Gurugram-Cyber-Heist-2021-Write-Ups
Solutions to the CTF conducted at GPCSSI 2021 - Cyber Heist 

Note: This CTF was not an international event, and it is not on ctftime.org either.

## 1. All About Web

```
Our heist has come to a position where we are stuck with the web interfaces but we need your help Visit https://chall.hackershala.com
```

* Site: https://chall.hackershala.com/
* SQLi was bypassed with `' or 1=1--` in both fields

* `CTF{YOUGOTTHEFLAG}` is the flag

![image](https://user-images.githubusercontent.com/43957261/126024976-f22bffe0-8516-4cb4-b0a8-c11332d16d1f.png)

* The other part was just `robots.txt`
* `CTF{YOUAREHACKER}` is the flag.

![image](https://user-images.githubusercontent.com/43957261/126025016-d04e32d2-e08e-4f03-9cf4-ca87e2b01950.png)


## 2. Not Just SQL

```
There have been a breakthrough in our heist but we still cant access chall2.hackingbrawl.com as a privileged user.
```

* As usual, there is a login page: https://chall2.hackingbrawl.com/newlogin.php (This challenge was the next in the sequel to the basic sqli one)

* At first few SQL injection methods were attempted, but these went in vain. Upon using sqlmap, it was discovered that the site was vulnerable to `time-based-blind` injection. So manual attempts were done, the site seemed to slow down a bit when the payload was injected. 

* The `data` value required for sqlmap was found upon doing a POST request. This value was: `enroll=a&passes=`

* the lower levels did not work, so the level was increased and tested.

* `hack_chall2` was the database, `TAB` was the table, with `PASSWORD` and `USERNAME` as field(s).

```bash
python3 sqlmap.py --url https://chall2.hackingbrawl.com/newlogin.php --technique=T --random-agent --data='enroll=a&passes=' --level=5 -D hack_chall2 --dump --no-cast
```

* The flag was in one of the rows of the table: `CTF{INJECTKAFLAG}`


![image](https://user-images.githubusercontent.com/43957261/125963329-f5b1d70d-ee8f-467e-a72b-c0ea361aeaa3.png)

![image](https://user-images.githubusercontent.com/43957261/125963577-14dd5f56-3b2b-4f94-bc28-7aefed537957.png)


## 3. Are you Web Expert?

```
The hacker is playing again with us but this time we need to be patient and logical. Are you ready? https://iopt3w.hackingbrawl.com/
```

* Ok, so the short answer to the question is: No, I'm not lol.
* Site: https://iopt3w.hackingbrawl.com/
* The only interesting thing is the cookie part, which says "admin". Now, encoding the cookie should work, provided there is no other hurdle.
* Upon encoding the cookie with base64, we get the flag.
* The flag: `ctf{thisistheflag}` 

![image](https://user-images.githubusercontent.com/43957261/126025887-7a9fcdae-9885-4d87-b98b-79c9a7023040.png)


## 4. Mobile Phones are Bad

```
The hacker said, Gurugram Interns are intelligent enough to get through this challenge. https://mudpmd.hackingbrawl.com/
```

* Again, it's a login page. https://mudpmd.hackingbrawl.com/

![image](https://user-images.githubusercontent.com/43957261/126001906-689cdcad-28ba-4a24-b3ae-af97cd55b685.png)

* SQLi was bypassed with a common payload.
* Then, https://mudpmd.hackingbrawl.com/validate_login.php asks us to provide otp. Luckily, at this point, guessing that the OTP would be a 4 digit OTP worked. (Bruteforcing seemed to be the only option now, with the numbers ranging from 0000-9999 -> num.txt, which is nothing but the wordlist)

![image](https://user-images.githubusercontent.com/43957261/126002275-627ad73c-e0bb-4d5f-8e47-cc8cb78676d1.png)

* `ffuf` was used with the PHPSESSIONID of the `validate_login` page.
```bash
ffuf -u https://mudpmd.hackingbrawl.com/validate_login.php -b 'PHPSESSID=41vmugnlapp6vhka6ak6teqjo3' -w num.txt -d 'code=FUZZ&btnValidate=' -H 'Content-Type: application/x-www-form-urlencoded' -fr Error
```
* `7621` is the OTP. 

![image](https://user-images.githubusercontent.com/43957261/125999210-b85ee607-6a4b-4405-935d-c5280e51f7d6.png)

* `ctf{youdidit}` is the flag

![image](https://user-images.githubusercontent.com/43957261/126000972-516193e2-80e6-467e-ae81-e0d0c362d33c.png)


## 5. The Last Step

The challenge:
```
This is the last step of the heist and some noobs will say it is difficult but mark my words its all about maths, numbers and a good programmer with a curious mindset.
CFF{POUAAABMEHXKFRSRCLKTG} is the flag. Key to every locker is often not given but if you are still curious https://www.linkedin.com/in/amanjiofficial/   (Points: 150)
Hint: If Aman Sir will ever get a chance, he will marry at Eiffel Tower.
```

* Eiffel Tower -> Vigenère Cipher
* Attempts were made to guess the key:
  * Tried with `amanjiofficial` -> no result
  * Eventually, I wrote a script, but it was taking a lot of time (for some reason)
  * Then `amanahuja` was tried, but still there was no clue of the flag.
  * Finally `amanahujaisthecreatoroft` worked, the flag was `CTF{CONGRATULATIONSYOUWON}`

* Verifying once the key is known is easy, although I wanted to use automation only for the prior part.

![image](https://user-images.githubusercontent.com/43957261/126024779-096dac92-04f6-4086-9d3e-76f720af1fa4.png)


## 6. Social Media Havoc

```
The heist has taken an interesting shape but reaching social media of the hacker is still unknown
```

* After logging in, we arrive at https://chall3.hackingbrawl.com/newlogin.php which says:
 
```Our server has been taken over by a Hacker and we have appointed you as the cyber security expert to catch the Hacker. 
In our initial investigation we found that the Hacker is very fond of using hashtags and we got a clue about him. 
Follow #hackershalahackershala group to get to the Hacker. One interesting fact about this social media platform is that it can do what even twitter cannot.
```
* `#hackershalahackershala` hashtag was found on Facebook. Comments did not have anything in particular.
* When I checked the edit history of the post, it led me to a rabbit hole(maybe this was to confuse the player).

![image](https://user-images.githubusercontent.com/43957261/126009644-17d70dc4-8823-4592-be96-1e150116211e.png)

* But the flag was somewhere else(in the edit history itself)

* `CTF{WOHOOOO}`
