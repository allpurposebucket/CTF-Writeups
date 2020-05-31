# **The 8-Bit Engagement**
## Points: 100
### **Description:** Dana was just discovered, downloading copywritten music from the internet after hours on her company issued laptop. Her favorite method of disguising data is using normal looking music files. [This one](https://capturetheflag.online/files/209629f3a2e2c16f4644b5d364aef4ff/8-bit_organizer.mp3) looked fishy could you find the hidden info ?

Listening to the MP3, it's clear to see that this is an 8-bit version of Billie Eillish's Bad Guy
I tried nearly everything I could think of when dealing with audio files. I tried using things like `strings`, `MP3Stego`, LSB stego, and others. Eventually, I researched tools to use
to detect and decode audo steganography, and I eventually found [AudioStego](https://github.com/danielcardeenas/AudioStego). Once I installed it, I simply ran `./hideme file.mp3 -f` and it returned this:

```
Doing it boss! 
Looking for the hidden message...
String detected. Retrieving it...
Message recovered size: 28 bytes
Message: 'HilltopCTF{1mm_the8ad_6uy}'
Cleaning memory...
```



## **Flag:** HilltopCTF{1mm_the8ad_6uy}
