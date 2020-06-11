# **Cut Short**
## Points: 200
### **Description:** [This image](files/flag.png) refuses to open in anything, which is a bit odd. Open it for the flag! 

Running `pngcheck` on the image tells us that the first chunk "Must be IHDR." All I did to fix the image was run [PCRT](https://github.com/sherlly/PCRT) on it, which is a tool made
to detect and repair corrupt parts of png files. That's all I did to find the flag. 

## **Flag:** ractf{1m4ge_t4mp3r1ng_ftw}
