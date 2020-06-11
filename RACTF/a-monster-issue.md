# **A Monster Issue**
## Points: 100
### **Description:** Agent, We've got a case of industrial espionage, quite an unusual one at that. An international building contractor - Hamilton-Lowe, has written to us that they are having their private client contracts leaked. After conducting initial incident response, they managed to find a hidden directory on one of their public facing web-servers. However, the strange thing is, instead of having any sensitive documents, it was full of mp3 music files. This is a serious affair as Hamilton-Lowe constructs facilities for high-profile clients such as the military, which means having building schematics leaked from them could lead to a lapse in national security. We have attached one of these [mp3 files](files/aero_chord.mp3), can you examine it and see if there is any hidden information inside?

After running binwalk on the mp3, I was given a wav file, which I opened in Sonic Visualiser. Once I pulled up a spectrogram of the audio, the string `password{Shad0ws}`

Running `7z e` on the files that I had with the password, I got the flag from unzipping the wav file.

## **Flag:** ractf{M0nst3rcat_In5tin3t}
