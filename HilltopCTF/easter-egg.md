# **Easter Egg Hunt**
## Points: 50
### **Description:** Secure in the Deep Blue is at it again! Go to their website and follow the rabbit trail to reach the password list to crack [these company documents](https://capturetheflag.online/files/4070fa064b9291c86e4199ed5130b0bd/UserFolder.zip).

The file we got is password protected, and our goal is to find the password list. 

After finding the [website](https://secureinthedeepblue.com), I looked around and found a [pastebin link](https://pastebin.com/YHkkEG9u) with some binary and decimal values around the values of printable ASCII characters. When converting all of this,
we get [Secureinthedeepblue.com/Shawn](https://Secureinthedeepblue.com/Shawn). This page is password protected, although the binary gives us "Secure." Entering this into the password form, it works. We're given [another pastebin link](https://pastebin.com/gzTuBjFY).
We repeat the process of converting the strings in the pastebin to find [another site](https://secureinthedeepblue.com/Raj). The password for the second one was _In_.

It took me a while to notice that there was a pattern. The passwords were constructing the url of the website. I was able to just go through all of the password forms easily, until I got to [this one](https://secureinthedeepblue.com/Anita).
On the pastebin with the /Anita page, there is also a string in an unknown encoding/encryption: _61f0d814f3da59b3_

Hash identifiers revealed that the hash was LM or MySQL type, although no password cracker I tried worked. I then went to [cyberchef](https://gchq.github.io/CyberChef/) and tried every encryption it had. 
Once I decrypted it using the RC2 hash function, it gave us /master. Entering this into the password field, we get the last pastebin link, which just shows the word "mind." 

Thus far, the passwords constructed the string *SecureInthedeepblue.com*. Adding /master + mind to the end, we get [this.](https://secureinthedeepblue.com/mastermind)

Submitting "SecureInthedeepblue.com/mastermind" into the password field gives us the password dictionary. I then used the command `fcrackzip -u -D -p PasswordList.txt UserFolder.zip` to find the password, which was _s3cure2020_. Unzipping the zip file with that password
This gives us a directory system. After looking through all of the empty files and directories, I found this in UserFolder/John/Work/Spring 2020/New Website/Ascii.txt.
![Easter Egg](../images/easter-egg.JPG)
## **Flag:** HilltopCTF{KxhPmCtjwV4dgkNFYAvAAkkryvR1U2Hu3i8BnZSca8MT1291ZGhW}
