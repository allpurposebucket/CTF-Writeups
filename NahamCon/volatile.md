# **Volatile**
## Points: 125
### **Description:** What was the flag again? I don't remember... You don't need the password. Download the file below. Note, this flag is not in the usual format.

Volatility is a tool used for memory forensics, which is examining a system or system image at a certain point in time and analyzing the memory. 
To run most of the functions with the tool, you need a "profile." To get suggested profiles for an image, you use imageinfo. From there, 
I ran `volatility -f memdump.raw --profile=Win7SP1x86_23418 cmdscan` and found the flag.

## **Flag:** JCTF{nice_volatility_tricks_bro}
