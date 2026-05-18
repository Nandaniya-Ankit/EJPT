### Gobuster:
Gobuster is a fast, open-source command-line tool used by cybersecurity professionals to brute-force and discover hidden files, directories, subdomains, and virtual hosts on web servers. It operates by rapidly sending requests based on custom wordlists to uncover unlinked resources or misconfigurations.

```
gobuster dir -u http://demo.ine.local -w /usr/share/wordlist/dirb/common.txt
```
`-u`: url
`w`: wordlist

Filtering response code with 403 and 404
```
gobuster dir -u http://demo.ine.local -w /usr/share/wordlist/dirb/common.txt -b 404,403
```
`b`: blacklist

#tip 
Seclist wordlist @ github

Filtering files with specific extension:
```
gobuster dir -u http://demo.ine.local -w /usr/share/wordlist/dirb/common.txt -b 404,403 -x .php,.xml,.txt -r
```
`-x`: specify file ext.
`r`: follow redirection

Scanning a directory
```
gobuster dir -u http://demo.ine.local/data -w /usr/share/wordlist/dirb/common.txt -b 404,403 -x .php,.xml,.txt -r
```

### Dirb
Directory BF
```
dirb url
```