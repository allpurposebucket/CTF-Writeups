# **Controlled Access**
## Points: 50
### **Description:** We've been asked to help a certificate authority figure out what a [device](https://shop.hak5.org/products/shark-jack) they found plugged into their network was doing. They were able to dump the [firmware](https://challenge.acictf.com/static/ba2f9d44ae1426891b3f2286544c4e50/firmware.bin) and would like to know if it allowed the attacker to connect to any devices that their firewall (which blocks inbound SSH) would have stopped. Their internal domain uses 'digisigner.local' for DNS host names. The flag is the hostname of the internal host that the hacker targeted (i.e. ACI{[local hostname targeted]}).

Running binwalk on the provided firmware reveals compressed data inside the binary. Exporting that data with `binwalk -e`, I found that the data was a filesystem.

One of the hints talks about how the 'attack' payload is stored in a very particular spot in the filesystem. Looking at the [documentation](https://docs.hak5.org/hc/en-us/categories/360002117973-Shark-Jack), it's
clear that the payload is in the /root/payload directory. Once we cd to the directory, we see payload.sh. If we view the contents of that file, our internal host (the flag) is shown.

## **Flag:** ACI{rootca.digisigner.local}
