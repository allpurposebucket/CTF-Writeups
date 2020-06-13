# **Fake File**
## Points: 100
### **Description:** Wait... where is the flag? Connect here: nc jh2i.com 50026

Running `ls` in this shell returns nothing, but we can find that there is a file named `..`. On closer look, we can determine that it's 
actually named `.. `, with a space, or another character. There are a couple commands that I got to work that printed that file. These include `cat ..*` and 
`grep -r ..*`

## **Flag:** flag{we_should_have_been_worried_about_u2k_not_y2k}
