
# Startup

IP - 10.10.149.162

## Scanning and Enumeration

nmap:
```
21/tcp open  ftp     vsftpd 3.0.3
| ftp-syst:
|   STAT:
| FTP server status:
|      Connected to 10.6.41.41
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 2
|      vsFTPd 3.0.3 - secure, fast, stable
|*End of status
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
| drwxrwxrwx    2 65534    65534        4096 Nov 12  2020 ftp [NSE: writeable]
| -rw-r--r--    1 0        0          251631 Nov 12  2020 important.jpg
|*-rw-r--r--    1 0        0             208 Nov 12  2020 notice.txt
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.10 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 b9:a6:0b:84:1d:22:01:a4:01:30:48:43:61:2b:ab:94 (RSA)
|   256 ec:13:25:8c:18:20:36:e6:ce:91:0e:16:26:eb:a2:be (ECDSA)
|_  256 a2:ff:2a:72:81:aa:a2:9f:55:a4:dc:92:23:e6:b4:3f (ED25519)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|*http-server-header: Apache/2.4.18 (Ubuntu)
| http-methods:
|*  Supported Methods: OPTIONS GET HEAD POST
|_http-title: Maintenance
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose
Running: Linux 4.X
```


dirsearch:
```
[14:21:55] 403 -   277B - /.php

[14:21:55] 403 -   277B - /.php3

[14:22:22] 301 -   312B - /files  ->  http://10.10.194.20/files/

[14:22:22] 200 -    1KB - /files/

[14:22:25] 200 -   808B - /index.html
```


Clue found in http://10.10.149.162/files/notice.txt


![image](https://github.com/user-attachments/assets/62099454-1c6d-49c7-ad40-118f62f32e98)



FTP port is accessible using "anonymous" username.


![image](https://github.com/user-attachments/assets/a262527a-6217-4034-a30b-4cacf208489d)



## PHP Reverse Shell


ftp directory have full permissions enabled which allowed me to transfer from my machine to the ftp server a php reverse shell file and execute it while having netcat listener in the background.


![image](https://github.com/user-attachments/assets/b0745e84-34c8-4568-a1a6-0169359f534b)


Successfully got a reverse shell!


![image](https://github.com/user-attachments/assets/e3d2aa87-1bb0-4c50-b9c2-60402b0a7ff6)


