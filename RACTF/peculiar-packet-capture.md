# **Peculiar Packet Capture**
## Points: 400
### **Description:** Agent, We have a situation brewing. Last week there was an attack on the prime minister of Morocco. His motorcade was stopped by a road blockade where heavily armed men opened fire on them. Fortunately, the prime minister was able to escape safely but many personnel and a few other ministers did not. ATLAS, a multi-national Private Military Corporation (PMC) based in Colorado, USA, is our main suspect. We believe they were hired to conduct the hit by the opposition political party. We flew Agent Jason to Colorado to investigate further. He gained access to their building's reception area dressed in a suit acting as a potential client with an appointment. He was able to intercept wireless network traffic from their corporate wireless network before being escorted out by guards when they realised the bluff. The network capture is [attached](images/ATLAS_Capture.cap), see if you can recover any important documents which could help us tie ATLAS to the Morocco incident.

Opening the pcap in wireshark, we find a bunch of encrypted traffic along with 4 EAPOL messages, which to my understanding is a 4-way handshake. 
To find the ESSID and password, I ran `aircrack-ng ATLAS_Capture.cap -w /usr/share/wordlists/rockyou.txt`. This gave me the password `nighthawk`. 
I then used airdecap to decrypt the traffic and produce a new packet capture file using the command `airdecap-ng -p nighthawk ATLAS_Capture.cap -e ATLAS_PMC`.

Next I opened the new pcap in wireshark, then exported all of the files that it could find, including a pdf. Once I opened the pdf, I found the flag.

## **Flag:** ractf{j4ck_ry4n}
