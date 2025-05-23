# AgentSudo

IP - 10.10.149.28

## Scanning and Enumeration
nmap:
```
21/tcp open  ftp     vsftpd 3.0.3
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 ef:1f:5d:04:d4:77:95:06:60:72:ec:f0:58:f2:cc:07 (RSA)
|   256 5e:02:d1:9a:c4:e7:43:06:62:c1:9e:25:84:8a:e7:ea (ECDSA)
|_  256 2d:00:5c:b9:fd:a8:c8:d8:80:e3:92:4f:8b:4f:18:e2 (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|http-server-header: Apache/2.4.29 (Ubuntu)
| http-methods:
|  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Annoucement
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
```
dirsearch:
```
dirsearch -

[21:37:02] 403 -   276B - /.php

[21:37:33] 200 -   218B - /index.php

[21:37:33] 200 -   218B - /index.php/login/

[21:37:48] 403 -   276B - /server-status

[21:37:48] 403 -   276B - /server-status/
```

## Exploit
- To access the site I used BurpSuite to capture the request.
- Modified User-Agent by typing letters "A,B,C,D,E,F...", received "302 Found" which indicates redirection.
![image](https://github.com/user-attachments/assets/d20a6e6d-646e-4023-8f93-0c0c60b91657)
- Added the location: "agent_C_attention.php" to the GET request and got accessed to the site.
![image](https://github.com/user-attachments/assets/fff21a02-49e4-45a2-a072-1d553ca25bd4)

## Brute Force Attack
1. We know FTP server is open.
2. We know user is Chris.
3. We know Chris got a weak password.

To brute force the FTP server I used Hydra - ``` hydra -l chris -P /usr/share/wordlists/rockyou.txt -t 4 ftp://10.10.149.28 ```
![image](https://github.com/user-attachments/assets/d47f74f6-51a5-4472-92cf-ebdfce7d30f8)

Login successful using "chris" as the username and "crystal" as the password.

![image](https://github.com/user-attachments/assets/e673c106-981a-4a1e-a7b6-20958b1b66a9)

Found 'Agent J' file

![image](https://github.com/user-attachments/assets/4384db87-6b60-41dc-8798-c4457a928ea5)


Downloaded the file to my machine

![image](https://github.com/user-attachments/assets/47f6dcd3-619f-4039-b531-ea1f603c800c)

![image](https://github.com/user-attachments/assets/01b9a433-bd67-4fea-9e04-6a85016fe016)

- Went back to the FTP server and moved both images to my machine.
- Found zip file inside one of the images by running - ``` binwalk -e cutie.png ```

![image](https://github.com/user-attachments/assets/73b97020-ae76-494b-88fa-66a6e3356521)

- Zip file is unreadable and needs to be exploit.
- Used zip2john to extract hashed passwords
- Used john to crack the password

![image](https://github.com/user-attachments/assets/acd5f02e-6a4a-43b6-9e20-28d241846988)






