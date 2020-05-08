# **DENIED**
## Points: 75
### **Description:** Sometimes websites are afraid of the terminator finding things out. http://challenge.acictf.com:27856 The flag is in flag.txt.
The provided link had many references to robots, so the robots.txt was the first place I looked.
There was an entry in the robots.txt that took us to a separate page meant for "maintenance". The only thing on the page was "*Result=*" 

Looking at the page source, we can clearly see a commented out section of code for a submission form on the page. I decided to try sending POST requests to the URL
mentioned in hopes of getting a helpful response, although I always received a 400 code (Bad request).

It turns out I was overthinking the problem. After the POST request attempts, I removed the comment HTML tags and typed `cat flag.txt` and received the flag.


## **Flag:** ACI{ccdcb229da85c8d0a6a239edb19}
