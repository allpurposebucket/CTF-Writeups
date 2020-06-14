# **Rotten**
## Points: 100
### **Description:** Ick, this salad doesn't taste too good! Connect with: nc jh2i.com 50034

This server sends a sentence, sometimes plaintext, sometimes shifted with a Caesar cipher. Random sentences given to us contain a character and an index of the flag
that that character represents. I wrote a python script to try every shift, sending back the sentence once it's decrypted, all while adding the characters to a dictionary with their indices. 

```python3
import socket
import time


ip = "jh2i.com"
port = 50034
sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
sock.connect((ip, port))
flagStr = ""
keepCharacters = "1234567890-_=+[]{}|,./?!@#$%^&*() '"
nums = "0123456789"
flagDict = {}
f = sock.makefile()
while True:
	resp = f.readline().strip()
	print(resp)
	if "send back" in resp:
		print("sending:------ " + resp)
		sock.send(resp + "\n")
	else:	
		for shift in range(1, 26):
			result = ""
			for i in range(len(resp)):
				char = resp[i]
				if char in keepCharacters:
					result += resp[i]
				else:
					result += chr((ord(char) + shift - 97) % 26 + 97)
			#print(result)
			if "send back" in result:
				res = [int(i) for i in result.split() if i.isdigit()] 
				if len(res) > 0:
					flagDict[res[0]] = result[-2]
				sock.send(result + "\n")
				print("sending:------" + result)
				
				
				break
			
	print(len(flagDict))

	if len(flagDict) > 30:
		for i in range(0, 31):
			flagStr += flagDict[i]
		print(flagStr)
```

## **Flag:** flag{now_you_know_your_caesars}
