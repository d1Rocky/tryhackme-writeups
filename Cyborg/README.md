
# Cyborg



Archive file was available to download under the Archive tab

![image](https://github.com/user-attachments/assets/21c77094-778a-4bae-a5e6-2a53dd71f7eb)


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
