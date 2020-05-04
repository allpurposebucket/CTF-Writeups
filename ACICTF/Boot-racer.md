# **Boot Racer**
## Points: 150
### **Description:** *coming soon*

We're given another floppy disk image that when booting with qemu displays to us `Your flag is at 0x7DC0    Well... it was`
This tells us that sometime during the timespan of booting till that text is printed, the flag is at that address. 
Booting the image in qemu with the `-s` and `-S` tags allow us to run a control server at *localhost:1234*. I then ran gdb and hooked it to the server
by typing `target remote localhost:1234`. This allows us to use gdb in conjunction with qemu. 

If we set a watchpoint with gdb at the address **0x7DC0** and then running it, we come to a stop, which means that the value/data at that address
was changed! When we dump the memory and view the value near the address, we can see that the flag format *ACI{* is starting to form. All we have to do now
is step through with gdb. I was careful to not step too far to the point that the flag was being deleted. 

## **Flag:** ACI{fast_dbg}
