
# Couch

nmap: 
```
22/tcp   open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.10 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 34:9d:39:09:34:30:4b:3d:a7:1e:df:eb:a3:b0:e5:aa (RSA)
|   256 a4:2e:ef:3a:84:5d:21:1b:b9:d4:26:13:a5:2d:df:19 (ECDSA)
|_  256 e1:6d:4d:fd:c8:00:8e:86:c2:13:2d:c7:ad:85:13:9c (ED25519)
5984/tcp open  http    CouchDB httpd 1.6.1 (Erlang OTP/18)
| http-methods:
|_  Supported Methods: GET HEAD
|_http-favicon: Unknown favicon MD5: 2AB2AAE806E8393B70970B2EAACE82E0
|_http-title: Site doesn't have a title (text/plain; charset=utf-8).
|_http-server-header: CouchDB/1.6.1 (Erlang OTP/18)
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose
Running: Linux 4.X
OS CPE: cpe:/o:linux:linux_kernel:4.4
OS details: Linux 4.4
Uptime guess: 0.005 days (since Wed Jun 11 22:54:06 2025)
Network Distance: 4 hops
TCP Sequence Prediction: Difficulty=264 (Good luck!)
IP ID Sequence Generation: All zeros
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

* CouchDB is a NoSQL database that uses TCP port 5984.


![image](https://github.com/user-attachments/assets/b71c408d-e6d4-4da1-a21b-3aa62fc1305f)


dirsearch:


![image](https://github.com/user-attachments/assets/29328a7a-e865-4b63-9e43-b79822ed2ac9)


## Exploit Using Metasploit

![image](https://github.com/user-attachments/assets/50280ce6-2bee-4a0b-bedc-1281a5cd7292)


- Found user flag


![image](https://github.com/user-attachments/assets/7345d18b-cb6c-4734-b26a-e22f090b8dd9)


- Found user atena password inside the web interface at http://10.10.202.26:5984/_utils/document.html?secret/


![image](https://github.com/user-attachments/assets/5b828a83-e2c2-47b1-abd1-5a5bd055c63e)


# Priviledge Escalation

- Found remote priviledge escalation using searchsploit. It allow to create an admin user in CouchDB.


![image](https://github.com/user-attachments/assets/1444e0cf-2746-408d-b52c-2c89fbe2a111)


- Successfully created admin user.


![image](https://github.com/user-attachments/assets/bb8d7a2b-b687-41f2-a300-4842354a288f)


