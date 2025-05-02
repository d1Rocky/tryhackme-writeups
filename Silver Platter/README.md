# Silver Platter

IP - 10.10.105.228

## Scanning and Enumeration
nmap scan:
```
PORT     STATE SERVICE    VERSION
22/tcp   open  ssh        OpenSSH 8.9p1 Ubuntu 3ubuntu0.4 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 1b:1c:87:8a:fe:34:16:c9:f7:82:37:2b:10:8f:8b:f1 (ECDSA)
|_  256 26:6d:17:ed:83:9e:4f:2d:f6:cd:53:17:c8:80:3d:09 (ED25519)
80/tcp   open  http       nginx 1.18.0 (Ubuntu)
|_http-title: Hack Smarter Security
|_http-server-header: nginx/1.18.0 (Ubuntu)
| http-methods: 
|_  Supported Methods: GET HEAD
8080/tcp open  http-proxy
|_http-title: Error
| fingerprint-strings: 
|   FourOhFourRequest: 
|     HTTP/1.1 404 Not Found
|     Connection: close
|     Content-Length: 74
|     Content-Type: text/html
|     Date: Fri, 02 May 2025 21:41:09 GMT
|     <html><head><title>Error</title></head><body>404 - Not Found</body></html>
|   GenericLines, Help, Kerberos, LDAPSearchReq, LPDString, RTSPRequest, SMBProgNeg, SSLSessionReq, Soc
```

In "Hack Smarter Security" website, I found information inside "Contact" tab - "Silverpeas" which can suggest on a website and also username: scr1ptkiddy.
- Went to 10.10.105.228:8080/silverpeas and got to a login page

## Exploit (Authentication Bypass)
Found silverpeas exploit - https://gist.github.com/ChrisPritchard/4b6d5c70d9329ef116266a6c238dcb2d
Based on those findings it says you can log in if you remove the password field and then the login attempt will (usually) succeed.
- Using Burpsuite, I set "Intercept on" and went back to the login page and tried to log in using scr1ptkiddy. Once I got the interception to BurpSuite I deleted the password from the request and then clicked "Intercept off" which gave me ACCESS to the website as user scr1ptkiddy.
![image](https://github.com/user-attachments/assets/91d971fa-a941-4749-9fc1-ba9180eff0b3) 
In scr1ptkiddy account, when opening “my notification” there is a link at the top that says - http://http://10.10.105.228/:8080/silverpeas/RSILVERMAIL/jsp/ReadMessage.jsp?ID=5
- All you need to do is change the ID to differernt number.
![image](https://github.com/user-attachments/assets/952ae943-eac6-412b-9730-85a9dffb9366)
- found in ID=6 this message: Username: tim & Password: cm0nt!md0ntf0rg3tth!spa$$w0rdagainlol


## Post-Exploitation (SSH Login)
```
sudo ssh tim@10.10.105.228
cat user.txt
```
- after finding the first flag, I looked up tim's id to see what kind of permissions he got
```
tim@silver-platter:~$ id tim
uid=1001(tim) gid=1001(tim) groups=1001(tim),4(adm)
```
I looked up online “linux adm group” and found: "Group adm is used for system monitoring tasks. Members of this group can read many log files in /var/log, and can use xconsole. Historically, /var/log was /usr/adm (and later /var/adm), thus the name of the group."
- Navigate to /var/log and used this command to get a password - grep -ir "password". This is what I found in the logs:
![image](https://github.com/user-attachments/assets/6a051a8a-bb59-411c-bff1-094e8bcfa970)
- Navigate to /home directory and ran - su tyler (to switch users) and pasted the password I got earlier which ended up giving me access to tyler.
- I checked if tyler is able to access the root flag but received "permission denied". I checked tyler's permissions and noticed he has 27(sudo) which allows him to switch to root user.
- With this information I ran "sudo su" and used tyler's password again and got ACCESS to root user. All I had to do from there was reading the root.txt flag



