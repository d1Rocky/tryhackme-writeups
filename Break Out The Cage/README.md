
# Break Out The Cage

IP - 


## Scanning and Enumeration

nmap:
```
PORT   STATE SERVICE VERSION
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
|*End of status
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|*-rw-r--r--    1 0        0             396 May 25  2020 dad_tasks
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 dd:fd:88:94:f8:c8:d1:1b:51:e3:7d:f8:1d:dd:82:3e (RSA)
|   256 3e:ba:38:63:2b:8d:1c:68:13:d5:05:ba:7a:ae:d9:3b (ECDSA)
|_  256 c0:a6:a3:64:44:1e:cf:47:5f:85:f6:1f:78:4c:59:d8 (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|*http-title: Nicholas Cage Stories
| http-methods:
|*  Supported Methods: POST OPTIONS HEAD GET
|_http-server-header: Apache/2.4.29 (Ubuntu)
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose
Running: Linux 4.X
```
dirsearch:
```
[16:59:32] 301 -   311B - /html  ->  http://10.10.176.62/html/

[17:00:07] 200 -   737B - /html/

[17:00:08] 301 -   313B - /images  ->  http://10.10.176.62/images/

[17:00:08] 200 -    4KB - /images/

[17:00:08] 200 -    2KB - /index.html

[17:00:22] 301 -   314B - /scripts  ->  http://10.10.176.62/scripts/

[17:00:22] 200 -    2KB - /scripts/
```
## Port 80

- Cage son is Weston
- Found information in /scripts/ directory


![image](https://github.com/user-attachments/assets/2021c32c-bb75-4ac0-add0-405a10202f53)


## Port 22
- ssh is not allowing to log in with random credentials


![image](https://github.com/user-attachments/assets/0fd37ddc-4d1e-4518-83f1-005e968474ab)

## Port 21

- ftp allowing annonymous login

![image](https://github.com/user-attachments/assets/e6ebd02c-446a-4219-a90a-b5cf615cae64)

- Found file called ```dad_tasks```
- Moved file to my machine ```wget --ftp-user=Anonymous ftp://10.10.176.62/dad_tasks```
- Opened file and found encrypted message

![image](https://github.com/user-attachments/assets/b381a85e-b18c-4e2a-baf5-f9362c92dc57)


## Decrypting an Unknown Cipher
- Used CyberChef and found out that by decoding it with base64 I get a decrypted Cipher

![image](https://github.com/user-attachments/assets/554b5041-7046-46fb-8a6d-13d894bbc3f6)


- Found out the Cipher is Vigenere. I decoded it using dcode.fr

![image](https://github.com/user-attachments/assets/2f878959-1491-48f7-82a5-1a30328fe2c5)

- Weston's password - Mydadisghostrideraintthatcoolnocausehesonfirejokes 


## SSH Login
- username: weston | password: Mydadisghostrideraintthatcoolnocausehesonfirejokes

![image](https://github.com/user-attachments/assets/72c5b35f-cb49-4ab6-a7cb-0db6dcf4eff6)


- Weston's root permissions
```
weston@national-treasure:/$ sudo -l
User weston may run the following commands on national-treasure:
    (root) /usr/bin/bees
```


- Used find / -type f -user cage 2>/dev/null to discover files within cage directory

![image](https://github.com/user-attachments/assets/a53147bb-f772-442f-91f7-e2054863d68f)

- ```/spread_the_quotes.py``` calls ```/opt/.dads_scripts/files/.quotes``` choosing randomly a quote.


## Priviledge Escalation


![image](https://github.com/user-attachments/assets/43161db1-0566-43cc-9d5b-a79c8dfcba44)

- To get it to echo the executalbe file - ```printf 'anything;/tmp/shell.sh\n' > /opt/.dads_scripts/.files/.quotes```

![image](https://github.com/user-attachments/assets/fef46213-21ef-47a0-9b3f-22aa961c1efa)

- Found important information inside /email_backup/ directory


![image](https://github.com/user-attachments/assets/471be8e9-4381-47d9-a203-ca3373fdccd8)

- Used dcode again but this time I input cipher: ```haiinspsyanileph``` | key: ```FACE```


![image](https://github.com/user-attachments/assets/a6e0d65e-a622-4427-9146-04053f205b0e)


- Found user flag


![image](https://github.com/user-attachments/assets/03b9d4ce-2a39-4cde-bbed-c46a7058a84b)




![image](https://github.com/user-attachments/assets/30fbbe26-ff07-4efc-baa4-db0995df702a)


- Found Root flag!


![image](https://github.com/user-attachments/assets/f46781d6-fc18-4469-800d-61d890a8d0aa)

