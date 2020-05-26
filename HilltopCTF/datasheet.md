# **Datasheet**
## Points: 25
### **Description:** Usually, we send our customers a PDF file. This time, the IT team catch a password protected file with the specs of the new microphone adapter, but we suspect that there is something else. Try to dig more about this one.

We're given this [zip file](https://capturetheflag.online/files/a94bd47a40ca9d32f46889d5a5347877/aurex.zip). Seems like another brute force attack. Running 
`fcrackzip -v -u -D -p /usr/share/wordlists/rockyou.txt aurex.zip` provides us with the password "qwerty123456." The `-u` helps with false positives, the
`-D` sets fcrackzip to _dictionary_ mode, and the `-p` allows us to select a dictionary. 

Inputting the password into the zip file, we're now given a pdf. Looking at the pdf, there doesn't seem to be anything obvious. Running exiftool on it reveals a base64 encoded
string under the _Creator_ tag. Decoding this, we have the flag.
## **Flag:** HilltopCTF{m3t4d4T4_1s_1nf0}
