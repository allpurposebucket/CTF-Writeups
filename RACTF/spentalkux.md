# **Spentalkux**
## Points: 300
### **Description:** Spentalkux 🐍📦

We're not given much, so I searched Spentalkux on google, which led me to a PyPi module. I installed the python module, opened a python console, and imported it. 
After importing it, things started getting printed. We got the string `Ztpyh, Iq iir'jt vrtdtxa qzxw lhu'go gxfpkrw tz pckv bc ybtevy... *ffiieyano*. New cikm sekab gu xux cskfiwckr bs zfyo si lgmpd://zupltfvg.czw/lxo/QGvM0sa6`. They also tell us the key is 10 characters long.
Next, I put it into a cipher identifier, which told me it was a vigenere. Spentalkux is 10 characters, so I tried decrypting it with that key, and we get this message: `Hello, If you're reading this you've managed to find my little... *interface*. The next stage of the challenge is over at https://pastebin.com/raw/BCiT0sp6`

Following the link, we get a hexdump, which decodes to a PNG file. Opening up the image, we're told that we should look back "into the past." 
It took me a while to think of this, but I went back to the PyPi website and found that there was an older version of the module, which I installed and imported.
Now we have another cipher, `JA2HGSKBJI4DSZ2WGRAS6KZRLJKVEYKFJFAWSOCTNNTFCKZRF5HTGZRXJV2EKQTGJVTXUOLSIMXWI2KYNVEUCNLIKN5HK3RTJBHGIQTCM5RHIVSQGJ3C6MRLJRXXOTJYGM3XORSIJN4FUYTNIU4XAULGONGE6YLJJRAUYODLOZEWWNCNIJWWCMJXOVTEQULCJFFEGWDPK5HFUWSLI5IFOQRVKFWGU5SYJF2VQT3NNUYFGZ2MNF4EU5ZYJBJEGOCUMJWXUN3YGVSUS43QPFYGCWSIKNLWE2RYMNAWQZDKNRUTEV2VNNJDC43WGJSFU3LXLBUFU3CENZEWGQ3MGBDXS4SGLA3GMS3LIJCUEVCCONYSWOLVLEZEKY3VM4ZFEZRQPB2GCSTMJZSFSSTVPBVFAOLLMNSDCTCPK4XWMUKYORRDC43EGNTFGVCHLBDFI6BTKVVGMR2GPA3HKSSHNJSUSQKBIE`.

Playing around with CyberChef, I used a long list of conversions to eventually find plaintext. The conversions were like this: `Base32 -> Base64 -> Gunzip -> Binary -> Hex -> Base85`

## **Flag:** ractf{My5t3r10u5_1nt3rf4c3?}
