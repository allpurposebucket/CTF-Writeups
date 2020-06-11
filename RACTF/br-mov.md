# **BR.MOV**
## Points: 400
### **Description:** https://youtu.be/zi3pLOaUUXs

All we're given is a youtube video of a man speaking numbers with images of barcodes flashing on the screen. I knew that I could've wrote a script
to pull the data from the barcodes, although I just used an online tool to do it for me. Decoding all of the barcodes, we get:

```
5WlndrAehA
8PdGSTvnaY
9zuPGubRMc
7cyqggztfa
6AqGoWfWwR
7JwvAOM{Px
4JIEbOEkws
5NDuG4sOeb
9chPBBYtfr
8iwkHVYpcf
7hVMGQe0xL
3vBdLvZLbB
2T3iNatxiU
5kNLb_eoyi
4AfAmLXyJo
4oFE4iSJmP
3ajdUBIXVe
4oAQnoJxEV
8SzMNoIa3j
9aaIBHbqls
2vsDNpidao
1}gfkrtfrm
```

Looking at that, there are both a { and a }, which means that our flag is in there somehow. I then found that for each line, the number represents the index of the character
in the line that is part of the flag. I wrote a quick python script because I was too lazy to do it manually.

```python
f = open("decoded", "r")
flag = ''
for line in f:
	indexNum = int(line[0])
	flag += line[indexNum]

print(flag)
```


## **Flag:** ractf{b4rc0d3_m4dn3ss}
