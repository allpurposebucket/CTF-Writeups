# **Bitcoin Scam**
## Points: 50
### **Description:** We just caught the Mastermind behind a crypto-investment scam. But before any of the accomplices realise that their boss is missing and draw everything from the account, we think that you might be able to help us get the private key and we can proceed to seize the funds. We got for you the website and old emails to his accomplices. (Flag format: HilltopCTF{private key}.
Attachments: [email01.eml](https://capturetheflag.online/files/c236781be6bf6ff29464cf9aff674772/email01.eml) || [email02.eml](https://capturetheflag.online/files/7d97011190a7595fd812e65c57e60451/email02.eml) || [email03.eml](https://capturetheflag.online/files/9b8a380c2b628c2617b584b52b5cc27f/email03.eml) || [homepage.zip](https://capturetheflag.online/files/43ef11f82b36ea1616f940da8079670c/homepage.zip)

Unzipping homepage.zip gives us a web directory. Opening the index.html, we see a QR code. Decoding this we get a string: `1AwgyWmSKwFSWeqUaE2spFEBrxY74vfqER`.
Since it's a bitcoin problem, I looked up the address on a blockchain explorer website, but didn't find anything of use. 

Looking at the emails, I saw that they all had similar sentences with certain words replaced with "XXXX". Comparing all three emails we can construct one large sentence of every word. From there I had this sentence: 

`assist congress detail gather silly afford father loyal chimney rely execute vicious force forget squeeze rate shell rubber roast similar mechanic apart demise enhance`
To be honest, I had no idea what to do with that... so I googled it! I was quickly returned with a website talking about "Crypto Chaos Words" and "BIP39".
I had never heard of these, although once I found [this website](https://iancoleman.io/bip39/), it made more sense. I simply insert the sentence into the Mnemonic form, and they calculate everything I need. 
I scroll through the page, not knowing what much of it meant, and then I see the "Derived Adresses" section. Each address in the list had similar format to the address we got at the beginning. 
I didn't see our address on the list, though, so I generated 2000 more with the website and used ctrl f to search for our address (1AwgyWmSKwFSWeqUaE2spFEBrxY74vfqER). I found the address with the corresponding private key, and that's our flag.

## **Flag:** HilltopCTF{KxhPmCtjwV4dgkNFYAvAAkkryvR1U2Hu3i8BnZSca8MT1291ZGhW}
