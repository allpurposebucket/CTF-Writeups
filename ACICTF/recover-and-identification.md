# **Recover And IDentification**
## Points: 150
### **Description:** Given two out of three disk images used in a RAID array, see if you can recover the data: [disk1.img.xz](https://challenge.acictf.com/static/aa529854d4314a6ed33825aaae2fe476/disk1.img.xz) [disk2.img.xz](https://challenge.acictf.com/static/aa529854d4314a6ed33825aaae2fe476/disk2.img.xz)

We're given two disks of a RAID 5 array, and we're told that we need to recover the data. After some research, I found out that you could assemble
a RAID 5 array with one disk missing by creating another disk. I initially tried creating my own, but I couldn't get that to work so I thought of 
copying one of the disk images since we wouldn't need it in the end. 

I had an issue using loop 0-2 so I used loop 50-52, I'm not sure why this happened. I made each image block devices by using `losetup loop# file.img`.
I first created the array by running `mdadm --create --verbose /dev/md0 --level=5 --raid-devices=3 /dev/loop{50,51,52}`.
I then stopped the array, `mdadm --stop /dev/md0`. Next, I removed the block device that was the copy of the other image file, in my case it was loop52.
`losetup -d /dev/loop52`. Finally, I reassembled the RAID 5 array with just the two images. `mdadm --assemble --run /dev/md0 /dev/loop50 /dev/loop51`

Now, all I had to do was mount it. Once mounted, I found that there were a list of .tar files. When decompressing them, I ran `strings file.tar |grep ACI` on each one and found the flag.

## **Flag:** ACI{e818c5339604b14506d0b3a439e}
