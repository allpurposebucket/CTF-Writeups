# **A Musical Mix-Up**
## Points: 200
### **Description:** One of our guys found a [strange midi file](files/challenge.mid) lying around on our servers. We think there might be some hidden data in it. See if you can help us out!

I was very excited about this one because I got the first blood on it, which means I was the first person to solve it.

At first, I tried opening it in Audacity and listening to the audio, but found nothing. I then tried converting the midi file into a csv so that I could try to find some patterns, but again, nothing.
Eventually I just opened it up with `bless`, a hex editor. I was looking at the ascii and found a _r_ and an _a_, both after capital letters. I kept looking and found the entire flag, each after capital letters.

I suppose I kind of got lucky with finding it. I later found out that somebody else found it by grabbing the note velocity for each note and then converting those into ascii characters.

## **Flag:** ractf{f50c13ty_l3vel_5t3g}
