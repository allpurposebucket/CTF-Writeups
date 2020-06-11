# **Acess Granted**
## Points: 450
### **Description:** Agent, Do you recall the C2 group exfiltrating data we were tracking last year? Well, as it turns out, we've managed to corroborate their activities with APT-47 nicknamed 'The Engineers'. Their operations span across a wide range of industries, including disrupting SCADA systems and stealing corporate data. Recently, they've been leaking unreleased tracks from various media groups. A Canadian firm, which suffered a fresh leak, has requested us to take a look. Over the past few days, our analysts have combed through network data trying to identify which computers or servers may have been compromised and used. In order to bypass detection from IDS/DLP signatures, we think they're somehow extracting these tracks by hiding them in existing music videos so it blends in with usual traffic. We've attached a video that we believe is going to one of their IP addresses, can you take a look?

The video that we're given is a music video. Running `foremost` and `binwalk` both reveal a png image that contain the string "password{guitarmass}." 

I tried many different tools in hopes of finding a file that took a password, although I couldn't find anything. I eventually came across a tool called TCSteg, a tool that can detect and produce hidden TrueCrypt volumes inside a container file, like a mp4.
I originally installed and tried TrueCrypt, but then found out that it's discontinued, but has a newer version called [VeraCrypt](https://archive.codeplex.com/?p=veracrypt). Mounting the file with the password `guitarmass`, reveals a .png file with the flag.

## **Flag:** ractf{Butt3rsn00k's_R3veng3}
