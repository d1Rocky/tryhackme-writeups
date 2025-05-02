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
- found in ID=6 this message:
![image](https://github.com/user-attachments/assets/952ae943-eac6-412b-9730-85a9dffb9366)

