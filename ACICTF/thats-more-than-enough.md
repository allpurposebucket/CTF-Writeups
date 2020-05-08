# **That's More Than Enough**
## Points: 100
### **Description:** We think Jolly Jeff is up to no good. See if you can find the hidden message in his [JPEG Jammer](http://challenge.acictf.com:30380/).

The webiste that we're given allows us to provide an image and gets us to choose which filters we would like to apply to the image. Once I do so and download the new image,
I ran exiftool on it, which revealed nothing. We're given a hint to look at [JPEG file format specification](https://en.wikipedia.org/wiki/JPEG_File_Interchange_Format#File_format_structure).

By using bless, a hex editor, we can determine that the file had the correct file signature for a jpg. On the wiki page, it talks about how *FFDA* in the hex of a jpg
means that that is the beginning of compressed image data. When we run binwalk on the image, we can see that there is another image inside of our jpg.
`binwalk -e jammed.jpg` didn't export the compressed image, so I used dd instead. I ran `dd if=jammed.jpg of=out.jpg ibs=1 skip=367` and we got an output called out.jpg.

Once we display the new image file our flag is revealed. 

## **Flag:** ACI{23ffec4d059e47032f9d6e81985}
