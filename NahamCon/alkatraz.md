# **Alkatraz**
## Points: 100
### **Description:** We are so restricted here in Alkatraz. Can you help us break out? Connect here: nc jh2i.com 50024

When we try running commands on this shell, we find that there is a flag.txt, although commands like `cat`, `strings`, `grep`, etc. 
aren't available. Since it's a bash shell, I ran `$(<flag.txt)` and got the flag. This command is basically running a command
with the name equal to the contents of flag.txt, and echoing that out. Since there is obviously no command named flag{...}, it tells us: `/bin/rbash: line 5: flag{congrats_you_just_escaped_alkatraz}: command not found`.
## **Flag:** flag{congrats_you_just_escaped_alkatraz}
