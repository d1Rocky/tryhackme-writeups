
# GamingServer

IP - 10.10.249.71


nmap:
```
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 34:0e:fe:06:12:67:3e:a4:eb:ab:7a:c4:81:6d:fe:a9 (RSA)
|   256 49:61:1e:f4:52:6e:7b:29:98:db:30:2d:16:ed:f4:8b (ECDSA)
|_  256 b8:60:c4:5b:b7:b2:d0:23:a0:c7:56:59:5c:63:1e:c4 (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
| http-methods:
|_  Supported Methods: GET POST OPTIONS HEAD
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: House of danak
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Aggressive OS guesses: Linux 4.15 (99%), Android 9 - 10 (Linux 4.9 - 4.14) (96%), Linux 3.2 - 4.14 (96%), Linux 4.15 - 5.19 (96%), Linux 2.6.32 - 3.10 (96%), Linux 5.4 (95%), Linux 2.6.32 - 3.5 (94%), Linux 2.6.32 - 3.13 (94%), Android 10 - 12 (Linux 4.14 - 4.19) (93%), Android 10 - 11 (Linux 4.14) (92%)
No exact OS matches for host (test conditions non-ideal).
Uptime guess: 14.163 days (since Thu May 29 19:36:49 2025)
Network Distance: 4 hops
TCP Sequence Prediction: Difficulty=257 (Good luck!)
IP ID Sequence Generation: All zeros
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

dirsearch:
```
[23:32:08] 403 -   277B - /.php

[23:32:13] 200 -    2KB - /about.php

[23:32:13] 200 -    1KB - /about.html

[23:32:39] 200 -    3KB - /index.html

[23:32:53] 200 -    33B - /robots.txt

[23:32:54] 200 -   940B - /secret/

[23:32:54] 301 -   313B - /secret  ->  http://10.10.249.71/secret/
[23:32:54] 403 -   277B - /server-status

[23:32:54] 403 -   277B - /server-status/

[23:33:00] 301 -   314B - /uploads  ->  http://10.10.249.71/uploads/

[23:33:00] 200 -    1KB - /uploads/
```


- Found ssh key inside ```/secret/``` directory


![image](https://github.com/user-attachments/assets/2c0085a9-4140-4f8c-90d8-a19a0b999e53)



- Found info in ```/uploads/``` directory


![image](https://github.com/user-attachments/assets/0daeadfa-58f8-4c67-8a85-72d417857501)


- Found a dictionary list inside ```/uploads/dict.lst/```



- Found user John (Ctrl+U) 


![image](https://github.com/user-attachments/assets/d460370a-be82-4444-8c07-63865fafc6c1)


## SSH Exploit


- Copy paste the RSA key into a file. Ran ssh2john in order to crack the passphrase


![image](https://github.com/user-attachments/assets/9394a64c-407f-4117-a554-49a30209055c)


- Successfully logged into john using SSH


![image](https://github.com/user-attachments/assets/06fd0ba6-dd25-43a9-a07f-895b22037789)


- Found user.txt


![image](https://github.com/user-attachments/assets/3267ac39-9cf2-48ea-8eec-950744e339ea)


