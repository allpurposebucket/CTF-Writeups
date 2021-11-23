# Arctic Writeup


## Enumeration

#### Port Scanning

```
Nmap scan report for 10.129.220.247
Host is up (0.046s latency).

PORT      STATE SERVICE VERSION
135/tcp   open  msrpc   Microsoft Windows RPC
8500/tcp  open  fmtp?
49154/tcp open  msrpc   Microsoft Windows RPC
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows
```



## Initial Access

Poking around at [http://10.129.220.247:8500/](http://10.129.220.247:8500/) revealed an Adobe Coldfusion login panel. I searched for exploits for this version, and found [this](https://www.exploit-db.com/exploits/50057) exploit.
Running this exploit gets me a user shell. 


## Privelege Escalation
I ran `wes` (Windows Exploit Suggester) against the systeminfo dump I got from my shell, and received a lot of privesc routes, a lot of which that didn't work. The one that did end up working was [Chimichurri](https://github.com/Re4son/Chimichurri).


