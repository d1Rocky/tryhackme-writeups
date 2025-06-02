


## Brute It

IP - 10.10.130.120


## Scanning and Enumeration

nmap:
```
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 4b:0e:bf:14:fa:54:b3:5c:44:15:ed:b2:5d:a0:ac:8f (RSA)
|   256 d0:3a:81:55:13:5e:87:0c:e8:52:1e:cf:44:e0:3a:54 (ECDSA)
|_  256 da:ce:79:e0:45:eb:17:25:ef:62:ac:98:f0:cf:bb:04 (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-title: Apache2 Ubuntu Default Page: It works
|_http-server-header: Apache/2.4.29 (Ubuntu)
| http-methods: 
|_  Supported Methods: GET POST OPTIONS HEAD
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose
Running: Linux 4.X
OS CPE: cpe:/o:linux:linux_kernel:4.15
OS details: Linux 4.15
Uptime guess: 42.742 days (since Sun Apr 20 03:32:53 2025)
Network Distance: 4 hops
TCP Sequence Prediction: Difficulty=260 (Good luck!)
IP ID Sequence Generation: All zeros
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

dirsearch:
```
[21:20:35] 403 -   278B - /.php                                             
[21:20:42] 301 -   314B - /admin  ->  http://10.10.130.120/admin/           
[21:20:43] 403 -   278B - /admin/.htaccess                                  
[21:20:43] 200 -   671B - /admin/
[21:20:43] 200 -   671B - /admin/index.php                                  
[21:21:06] 200 -   11KB - /index.html     
```


When looking at source code (ctrl+U) at http://10.10.130.120/admin/ - I found username: admin


![image](https://github.com/user-attachments/assets/fee3283c-f60a-40c6-88e5-65763f628f5c)



## Brute Force (Hydra)

Used Hydra to brute force password

![image](https://github.com/user-attachments/assets/007c0e11-e48a-4d89-bbac-d53b85237c4d)


Login successful and found web flag and RSA Key


![image](https://github.com/user-attachments/assets/30fe73d2-f819-4f82-ac6a-73dae8260a43)


![image](https://github.com/user-attachments/assets/9edec0fa-27b6-48b4-a9ce-042b7c5a8e8a)


## SSH Exploit

- SSH is being prevents since they ask for passphrase

![image](https://github.com/user-attachments/assets/97b6a84a-c9a7-4fd5-b7d5-bb58fc11dc15)


- To fix this, I took the RSA Key and converted the encrypted SSH Key to a Hash Format using ssh2john.py

![image](https://github.com/user-attachments/assets/ee376b29-4690-4aa3-b896-def8d7533f4f)


- Then I used john to crack the hash

![image](https://github.com/user-attachments/assets/a27352e8-182e-4ad6-82f2-42e1ebbb747c)

