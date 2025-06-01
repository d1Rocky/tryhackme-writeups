
# Cyborg

IP - 10.10.102.117


# Scanning and Enumeration
nmap:
```

22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.10 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 db:b2:70:f3:07:ac:32:00:3f:81:b8:d0:3a:89:f3:65 (RSA)
|   256 68:e6:85:2f:69:65:5b:e7:c6:31:2c:8e:41:67:d7:ba (ECDSA)
|_  256 56:2c:79:92:ca:23:c3:91:49:35:fa:dd:69:7c:ca:ab (ED25519)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
| http-methods:
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Apache2 Ubuntu Default Page: It works
|_http-server-header: Apache/2.4.18 (Ubuntu)
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose
Running: Linux 4.X
OS CPE: cpe:/o:linux:linux_kernel:4.15
OS details: Linux 4.15
Uptime guess: 41.382 days (since Sun Apr 20 11:40:25 2025)
Network Distance: 4 hops
TCP Sequence Prediction: Difficulty=261 (Good luck!)
IP ID Sequence Generation: All zeros
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

```

dirsearch:

```

[20:50:06] 301 -   314B - /admin  ->  http://10.10.102.117/admin/

[20:50:07] 200 -    6KB - /admin/

[20:50:07] 403 -   278B - /admin/.htaccess
[20:50:07] 200 -    5KB - /admin/admin.html

[20:50:07] 200 -    6KB - /admin/index.html

[20:50:26] 200 -   928B - /etc/

[20:50:26] 301 -   312B - /etc  ->  http://10.10.102.117/etc/

[20:50:31] 200 -   11KB - /index.html
```



Found clue that was left by Alex to his team and archive file was available to download under the Archive tab

![image](https://github.com/user-attachments/assets/d6f2113e-7015-4f30-a985-9f74341a70a2)



Found information inside this directory - http://10.10.208.252/etc/squid/
```
music_archive:$apr1$BpZ.Q.1m$F0qqPwHSOG50URuOVQTTn.
```
```
auth_param basic program /usr/lib64/squid/basic_ncsa_auth /etc/squid/passwd
auth_param basic children 5
auth_param basic realm Squid Basic Authentication
auth_param basic credentialsttl 2 hours
acl auth_users proxy_auth REQUIRED
http_access allow auth_users
```

- The content of archive.tar was extracted with tar
- The folder contained information about "Borg Backup Repository"


![image](https://github.com/user-attachments/assets/345500b4-fbfd-444b-8a00-fc81dcc13424)



