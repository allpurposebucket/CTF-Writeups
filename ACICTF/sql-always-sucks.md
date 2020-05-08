# **SQL Always Sucks**
## Points: 150

This challenge gave us a website with a simple form that allowed us to input something and it would print it back to us. I first used Burp Suite to
get a request file with the injection point. The source code of the website tells us that it's using sqlite, so that narrows down our attempts (or sqlmap's attempts, rather). 

I then used sqlmap to dump the entire database. It took nearly 30 minutes although it gave me the flag. I ran this command: `sqlmap file.req --dbms=sqlite --tamper=space2comment,space2dash,space2plus
--dump`

Our flag eventually gets printed from the table SuperSecretData.

## **Flag:** ACI{6a0eae69fda11c5820a563217cd}
