# **Something Smells Phishy**
## Points: 25
### **Description:** You have received an email from your Project Manager. However, it looks out of the ordinary. Investigate the source of the [email](files/Urgent_Request.eml).

Extracting the .eml doesn't return anything. Binwalk doesn't return anything. The only thing we can do is mess around with the emails that it gave us.  These are hax@wendigo.pw and DM5PR18MB2088484FBE437D77AD0C4AD8B0D10@DM5PR18MB2088.namprd18.prod.outlook.com. The latter probably wouldn't tell us much, so I worked on finding out about wendigo.pw. There was no website running, although that doesn't mean that the domain doesn't exist. After trying nearly everything I could, I ran `dig any wendigo.pw` and found that they added a TXT record containing the flag.

## **Flag:** HilltopCTF{Ph1shy}
