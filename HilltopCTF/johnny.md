# **Johnny**
## Points: 25
### **Description:** There's this [zip file](https://capturetheflag.online/files/230ad9af9fac3c2ed88972cc832c38fe/flag.zip) with your flag in it. Crack the password to pass this challenge. 

We're told to crack the password without given any hints as to what it may be, so it's safe to assume it's in an accessible wordlist, so rockyou is a safe place to start. 
I first ran `zip2john` on the zip file. This generates a password hash from the zip file in a suitable format to give to John the Ripper. Once we have our hash(es), we simply run 
`john hashes`. Once this finished, the password "patricia" was revealed. Entering this password into the zip file reveals our flag.txt. 
## **Flag:** HilltopCTF{GIMME_DA_POWAH!__f5a6b3c4d5f933597f099e6686a39eeda26c8d3f1fe5416cd4ce19d9206d6c74}
