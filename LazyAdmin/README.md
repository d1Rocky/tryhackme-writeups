# Lazy Admin

IP - 


## Scanning and Enumeration

nmap -
```
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 49:7c:f7:41:10:43:73:da:2c:e6:38:95:86:f8:e0:f0 (RSA)
|   256 2f:d7:c4:4c:e8:1b:5a:90:44:df:c0:63:8c:72:ae:55 (ECDSA)
|_  256 61:84:62:27:c6:c3:29:17:dd:27:45:9e:29:cb:90:5e (ED25519)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-title: Apache2 Ubuntu Default Page: It works
|http-server-header: Apache/2.4.18 (Ubuntu)
| http-methods:
|  Supported Methods: GET HEAD POST OPTIONS
```


dirsearch - 
```
[14:06:23] 403 -   278B - /.php

[14:06:23] 403 -   278B - /.php3

[14:06:46] 301 -   316B - /content  ->  http://10.10.183.245/content/

[14:06:47] 200 -    2KB - /content/

[14:06:54] 200 -   11KB - /index.html

[14:07:09] 403 -   278B - /server-status

[14:07:09] 403 -   278B - /server-status/
```


login page

![image](https://github.com/user-attachments/assets/51bafbb8-adff-44f7-a2a5-12551b7fbb69)

