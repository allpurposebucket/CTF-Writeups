# **Turtles All the Way Down**
## Points: *coming soon*
### **Description:** *coming soon*

Opening the .pcap in Wireshark reveals that there is IRC packets being sent. In the IRC chat log, they discuss an "encrypted zip of the file capture." 
They also say that the password to the .zip is "dronehack2019"

When exporting the http objects with wireshark, we get the discussed zip file. We extract it using the provided password. Now we have another packet capture
file, this one containing mostly TCP and FTP traffic. When viewing the different TCP streams, we eventually find the flag text.

## **Flag:** ACI{334661b88dc8319b14c0788f80d}
