


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

- SSH asks for passphrase which I do not have

![image](https://github.com/user-attachments/assets/689c0f47-b79b-4c42-97a8-f96ea4b31072)



- To fix this, I took the RSA Key and converted the encrypted SSH Key to a Hash Format using ssh2john.py

![image](https://github.com/user-attachments/assets/ee376b29-4690-4aa3-b896-def8d7533f4f)


- Then I used john to crack the hash

![image](https://github.com/user-attachments/assets/a27352e8-182e-4ad6-82f2-42e1ebbb747c)


Successfully logged into John using SSH and passphrase: ```rockinroll```



![image](https://github.com/user-attachments/assets/554dd1d1-7a4e-4287-95f4-4245365dce67)


Found user.txt


![image](https://github.com/user-attachments/assets/6410d394-606f-45a2-b695-92ef74f28e5b)


- Found that user John can run the following command - 
```
User john may run the following commands on bruteit:
    (root) NOPASSWD: /bin/cat
```

- Found root hashed password in /etc/shadow

![image](https://github.com/user-attachments/assets/2d5f34c0-ec5b-435c-9d45-e2cafd0aa5ad)


- I cracked the hash using john the ripper

![image](https://github.com/user-attachments/assets/42438f9b-5a01-4b00-8b18-aaea5592185d)


- To get root.txt you can do two things:


1). you can use the password given to you and log into the machine as root and read the file.


2). Or you can use this command to read the file from john's machine - sudo /bin/cat /root/root.txt


![image](https://github.com/user-attachments/assets/a8f81e3d-f896-49ed-bda8-c01d55f1e2cc)




