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
