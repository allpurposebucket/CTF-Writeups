# **Boot Riddle**
## Points: 100
### **Description:** This floppy disk [image](https://challenge.acictf.com/static/756af1e5d64b939526e33e4d073d539c/files.tar.gz) boots, but instead of a flag we see some silly riddle...

We're given a floppy disk image that when booting with qemu doesn't give us much. 
Using the monitor tab in qemu and dumping the guest memory to a file, I then ran `strings guestMemory | grep ACI` to search the dumped memory for the flag format.
This reveals our flag.

## **Flag:** ACI{REALmode}
