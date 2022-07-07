# Recon

## Set target variables
```shell
export URL="http://target:80"
```

```shell
export IP="10.10.0.1"
```

```shell
export SESSION="token"
```

## Nmap
```shell
nmap -p- -sV -sS -Pn -A $IP
```

## Fuzzing with Wfuzz

### Directory discovery
```shell
wfuzz -c -z file,/usr/share/seclists/Discovery/Web-Content/raft-large-directories.txt --hc 404,301 "$URL/FUZZ/"
```

### Authenticated directory discovery
```shell
wfuzz -c -z file,/usr/share/seclists/Discovery/Web-Content/raft-large-directories.txt --hc 404 -b "PARAM=value" "$URL/FUZZ/"
```

### File discovery
```shell
wfuzz -c -z file,/usr/share/seclists/Discovery/Web-Content/raft-large-files.txt --hc 301,404 "$URL/FUZZ"
```

### Authenticated file fuzzing
```shell
wfuzz -c -z file,/usr/share/seclists/Discovery/Web-Content/raft-large-files.txt --hc 301,404,403 -b "PARAM=value" "$URL/FUZZ"
```

### Parameter discovery
```shell
wfuzz -c -z file,/usr/share/seclists/Discovery/Web-Content/burp-parameter-names.txt --hc 404,301 "$URL/FUZZ=data"
```

### GET parameter values
```shell
wfuzz -c -z file,/usr/share/seclists/Usernames/cirt-default-usernames.txt --hc 404,301 "$URL/index.php?parameter=FUZZ"
```

### html_escape fuzzing
```shell
wfuzz -c -z file,/usr/share/wordlists/Fuzzing/yeah.txt "$URL/FUZZ"
```

## Gobuster

### Endpoint discovery
```shell
gobuster dir -u $URL -w /usr/share/wordlists/dirb/common.txt
```

### Subdomain discovery
```shell
gobuster dns -d $URL -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt
```

## Hakrawler
#### Crawling 
```shell
echo "$URL" > urls.txt
cat urls.txt | hakrawler
```
