# **Trapped**
## Points: 100
### **Description:** Help! I'm trapped! Connect here: nc jh2i.com 50019

This shell doesn't allow us to do much of anything. It doesn't return any output besides telling us that we're "trapped." Looking up "bash trap" on google shows
us that there is a command named `trap` that allows us to automatically run a command when a signal is received. 

Running the command `trap "cat flag.txt" err exit` gives us the flag.

## **Flag:** flag{you_activated_my_trap_card}
