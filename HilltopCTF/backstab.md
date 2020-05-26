# **Backstab**
## Points: 50
### **Description:** Hello. I'm here to ask you a favor. It seems like an old co-worker of mine has been leaking some information about me on the internet. Here's his website: http://shellserver1.hilltopctf.masterwayz.nl:5843 Can you try to find a way to get the flag for me? I believe his username was 'jeffrey'.

When visiting the provided link, all we see is "Nothing to see here." Since this is clearly not enough information, 
I used `dirb` to find subdomains of the website. Dirb provided me with /secure and /passwd. When visiting /secure, there is an htttp authentication popup. 
When visiting /passwd, we download a hash (probably for /secure). Using an online hash analyzer, I found that the hash was a bcrypt hash. 

Running `hashcat -m 3200` with rockyou.txt reveals that the decrypted password is "kittycat." When logging into /secure with the credentials _jeffrey_
and password _kittycat_, the login goes through but still, all we see it "Nothing to see here." Fuzzing the /secure domain, we find there is /secure/flag.
When visiting that site, we find the flag.



## **Flag:** HilltopCTF{D0Nt-F0RG3T-CR3D5_ad45e1cb66483c46ee188fff223c7de1ef90213b93d2f1b1d5836beba06f487180d4417c869c4cade0d08ff216d2e24a39718ade3d87d0f06d8f184f6ae8c982}
