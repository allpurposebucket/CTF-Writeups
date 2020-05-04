# **Boot Riddle**
## Points: *coming soon*
### **Description:** *coming soon*

We're given a floppy disk image that when booting with qemu doesn't give us much. 
Using the monitor tab in qemu and dumping the guest memory to a file, I then ran `strings guestMemory | grep ACI` to search the dumped memory for the flag format.
This reveals our flag.

## **Flag:** ACI{REALmode}
