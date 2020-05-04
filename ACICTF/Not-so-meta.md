# **Not So Meta**
## Points: 50
### **Description:** Look, it's the flag! Oh wait...it looks like we need to take a closer look... not_so_meta.jpg
Running exif on the file didn't reveal any stored EXIF data, although we can clearly see that there is EXIF data inside when we run a hexdump on it.
Running exiftool revealed an encoded string assigned to the "It's The Flag" marker in base64. When decoding to ascii, we get the flag.
## **FLAG:** ACI{5f34564b3eb697b1630792161fa}
