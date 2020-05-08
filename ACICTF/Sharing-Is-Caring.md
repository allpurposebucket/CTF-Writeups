# **Sharing is Caring**
## Points: 250

This was a tough one. We're told that the challenge relates to [split-secret-schemes](https://en.wikipedia.org/wiki/Shamir%27s_Secret_Sharing). 
From this, I knew that I would be looking for a set of "secrets," which I expected to be long strings that I combined to reveal the flag.

We're given a zip file that contains 5 .png files. There wasn't anything fishy about them when runnning basic checks on them,
although after running **binwalk** on them, I found that there were .zlib files inside of them. After adding the `-e` tag to the binwalk call, I 
extracted the .zlib files from each .png. For images 1, 3, and 5, I found a similar sized value *after* the IEND marker, which I believe is supposed to be the
very last line of a .png file. I discovered that pngcheck would have told me that there is an extra chunk after the IEND marker, however I didn't
think about using pngcheck. I'm not sure if it was necessary at all to grab the .zlib files with binwalk, although it worked.

We're told that we need at least 4 of the 5 secrets. For images 2 and 4, the secret wasn't at the end of the files like the other 3 were. Instead, 
running exiftool on them revealed that they were the stored in the "Artist" tag in the metadata. 

Now we have 5 out of the 4 necessary flags. After researching ways to reconstruct the secret from the found flags, I decide to try seemingly the most 
direct route by using ssss-combine, a linux command specifically for split-secret-sharing-schemes. After attempting this command for a very long time
using different tags, nothing worked. ssss-combine wouldn't accept secrets that were different lengths or different "security levels"

From here, I didn't really know what to do, I tried padding the shorter secrets with values 0-9, which obviously didn't work. I eventually searched around
for a different way to combine them, trying multiple github repositories that also didn't work. I eventually found one that did work, [linked here.](https://github.com/timtiemens/secretshare)

```
~#java -jar secretshare.jar combine -k 4 -s5 889811123517623370368330373064098268573702381012314678901323646422235557755233727308934526544297623429045993145130701872340133705097974198620520314583954733 
-s3 233405028938806492297636443369806756737088423883542362209111476128505750713974379041588966381164174467046941723546730255723791664004659714921283125050611391 
-s4 490954926737699130471220227900391502772533285669433359190877128831830839590950838011884411655438244232442460056051863864567715340629055507170548621865963293 
-s1 18786878512788745873830654777289469073331705035606813377262882446691156899187330252458249424123064136053325675649521435763187595426983824346186898530334353

Secret Share version 1.4.5-SNAPSHOT
secret.number = '29519221060115231574023541916133953418825117386814042477224181014258526663293'
secret.string = 'ACI{2b1cfec1bc2f113a882d6d1a4c2}'
```

## **Flag:** ACI{2b1cfec1bc2f113a882d6d1a4c2}
