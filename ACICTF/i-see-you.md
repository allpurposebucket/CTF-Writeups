# **I SEe You**
## Points: *coming soon*
### **Description:** *coming soon*

We're given an audit.log file and we're told to find the attacker's IP address. I've never messed with an audit.log before, so I tried aureport 
first but didn't find anything notable. I then used ausearch with the `-i` tag to find all numbers in the file, generating a new file with all of the IP
addresses and actions taken from those addresses. I used grep to look for things that I would consider dangerous, like a streak of incorrect logins and 
things like that, although I never found anything.

I eventually was given [this website](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/6/html/security_guide/sec-Audit_Record_Types) from somebody that finished the challenge that lists all of the types used in the audit.log records.
I used grep to search each type I thought would have an important output, and evenutally found the attacker's IP address by searching for EXECVE, which allows a user to execute the program that is passed to it. In the 
attacker's case, he was running a nc command. 

## **Flag:** ACI{44.68.139.241}
