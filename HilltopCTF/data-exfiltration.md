# **Data Exfiltration**
## Points: 100
### **Description:** We managed to find an unsanctioned copy of our databases in the computer that belongs to one of our associates. Checking the timestamp, shortly after the creation date, this user sent an email to a public email address. Confirm the data exfiltration by finding the watermark (FLAG) in [the email.](https://capturetheflag.online/files/dfa5705688149fc41a1aabdf6e1bc3df/Financial_Services_and_Investment.eml)

Running `munpack` on the file, we get a .pptx and a .md5. Looking at the md5, it provides a file path, to "image9.jpg." 

A ppt is just a zip file in disguise. Running unzip on the powerpoint gives us a directory of everything that was in the powerpoint. We're also given a [Content_Types].xml file, which when viewed,
gives us a password: _crypto_volatility_high_. 

When we go into the directories, we find a media folder with all of the images. Running
`steghide extract -sf image9.jpg` with the password we found gives us a file named "clients.csv.gpg." Running `file` on this tells us that it is "GPG symmetrically encrypted data."
Looking up how to extract this data, I ran `gpg --decrypt clients.csv.gpg > foo` with the password found previously. This gave a list of names with credit card information. Running `grep CTF foo` gives us the flag.



## **Flag:** HilltopCTF{4A11EC495EE134DEFFFB63DAD54DABFF}
