# **Injection**
## Points: 25
### **Description:** For this one, you'll have to capture the flags spread in a database. Access is [here](http://shellserver1.hilltopctf.masterwayz.nl:3847/).

All we have is a form that searches a SQL table. The site shows us our SQL query. 

First, I used Burp to get the packet request file, then used sqlmap to dump the table. The command went like this: `sqlmap -r request.req --dump`
This returns three tables, each with a part of the flag. 


## **Flag:** HilltopCTF{Syr1ng35-L0v3_0623185b2ad227bb7a7d72d798b8adcd54818f54c8d64323bb3253f0}
