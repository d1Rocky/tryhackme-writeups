
# Wgel CTF

nmap: 
```
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 94:96:1b:66:80:1b:76:48:68:2d:14:b5:9a:01:aa:aa (RSA)
|   256 18:f7:10:cc:5f:40:f6:cf:92:f8:69:16:e2:48:f4:38 (ECDSA)
|_  256 b9:0b:97:2e:45:9b:f3:2a:4b:11:c7:83:10:33:e0:ce (ED25519)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-title: Apache2 Ubuntu Default Page: It works
| http-methods: 
|_  Supported Methods: POST OPTIONS GET HEAD
|_http-server-header: Apache/2.4.18 (Ubuntu)
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Aggressive OS guesses: Linux 4.15 (98%), Linux 4.4 (97%), Linux 3.2 - 4.14 (96%), Linux 4.15 - 5.19 (95%), Linux 3.10 - 3.13 (95%), Linux 2.6.32 - 3.10 (95%), Linux 3.10 - 4.11 (94%), Linux 3.8 - 3.16 (93%), Android 9 - 10 (Linux 4.9 - 4.14) (93%), Linux 3.13 (93%)
No exact OS matches for host (test conditions non-ideal).
Uptime guess: 15.724 days (since Thu May 29 00:02:37 2025)
Network Distance: 4 hops
TCP Sequence Prediction: Difficulty=264 (Good luck!)
IP ID Sequence Generation: All zeros
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

dirsearch:
```
[17:30:09] 200 -   11KB - /sitemap/.sass-cache/                             
[17:30:09] 200 -   954B - /sitemap/.ssh/                                    
[17:30:09] 301 -   319B - /sitemap/.ssh  ->  http://10.10.230.22/sitemap/.ssh/
[17:30:10] 200 -    2KB - /sitemap/.ssh/id_rsa                              
[17:30:13] 200 -   12KB - /sitemap/about.html                               
[17:30:31] 200 -   10KB - /sitemap/contact.html                             
[17:30:32] 301 -   318B - /sitemap/css  ->  http://10.10.230.22/sitemap/css/
[17:30:36] 301 -   320B - /sitemap/fonts  ->  http://10.10.230.22/sitemap/fonts/
[17:30:39] 200 -    8KB - /sitemap/images/                                  
[17:30:39] 301 -   321B - /sitemap/images  ->  http://10.10.230.22/sitemap/images/
[17:30:39] 200 -   21KB - /sitemap/index.html                               
[17:30:41] 200 -    4KB - /sitemap/js/                                      
[17:30:41] 301 -   317B - /sitemap/js  ->  http://10.10.230.22/sitemap/js/
```


![image](https://github.com/user-attachments/assets/e517a647-16ed-424e-b934-12cb1f341238)


I checked the source code and found a name - Jessie


![image](https://github.com/user-attachments/assets/7a4dde23-4f09-4fb0-8b84-d47ebad5f099)


In /sitemap/.ssh/id_rsa I found rsa key


![image](https://github.com/user-attachments/assets/b8977db9-4cfc-41a8-84e8-069cd0316118)



- I created a new file and pasted the rsa key and then changed its permissions by running this command - ```chmod 600```
- Tried SSH using this command - ```ssh -i [rsa-key] jessie@10.10.87.204```


![image](https://github.com/user-attachments/assets/535a6e46-c437-4d3a-88df-0ea57c9f0b27)



Found user flag in Jessie's Documents


![image](https://github.com/user-attachments/assets/eb4aacb0-e167-46d3-8cea-176ea3a6985c)


## Exploit

Found which command Jessie can run as root


![image](https://github.com/user-attachments/assets/121a309f-be14-4904-9474-2f7049424191)


Found Root flag

![image](https://github.com/user-attachments/assets/7b15236f-d1dc-466c-8794-17966a4aac34)


