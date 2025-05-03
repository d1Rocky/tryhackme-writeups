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


