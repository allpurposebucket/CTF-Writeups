# **Shady**
## Points: 25
### **Description:** We've got 2 files and 1 Eminem super-fan. Now you've got 1 password to crack. [passwd](files/passwd) || [shadow](files/shadow) || [note_to_self.txt](files/note_to_self.txt)

My first idea was just to make a wordlist from the Eminem wiki, although that didn't work. I then made a wordlist of every song by him with the spaces removed. In the note_to_self.txt file, it say that they'll just append their birthday onto the password. I didn't know the format of the dates, so I made a python script that tried appending nearly every format of date to each string in the wordlist. After running the password cracker, I found that the password was in format 101, 102 for January 1st and January 2nd, so I edited my python script into this: 

```python
datesList = []

with open("SongList.txt", 'r') as f:
	f = f.readlines()

f = map(lambda s: s.strip(), f)
		
o = open("EminemWordlist.txt", 'w')
#g = o.readlines()
for day in range(1, 32):
	if day < 10:
		day = "0" + str(day)
	else:
		day = str(day)
	for month in range(1, 13):
		date = str(month) + day
		datesList.append(date)

for line in f:
	for dates in datesList:
		new = line + dates + "\n"
		o.write(new)
```

I then grabbed the hash from the _shadow_ file and found that it was a sha512crypt hash. Next, I ran `hashcat -m 1800 --force hash EminemWordlist.txt` and gave back the password __TheWayIAm812__.

## **Flag:** HilltopCTF{TheWayIAm812}

