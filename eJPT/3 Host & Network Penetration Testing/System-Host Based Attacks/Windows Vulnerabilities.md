1. Frequently Exploited Windows Service

	-> _MS IIS_ (win web server) - PORT 80/443
	-> _WebDAV_
	(extension which helps a web server to act as a FILE Server) PORT 80/443.
	-> _SMB/CIFS_ - File Sharing Protocol -445
	-> _RDP_ - Disabled by default - 3389
	-> _WinRM_ - windows remote management protocol - 5986/443 

# Exploiting WebDAV
###### MS IIS  #IIS 
-> web server to host a web application , 
-> supports `.asp` `.aspx` `.config` `.php`

###### WebDAV
-> It is an extension , which help a web server (`MS IIS`) act as a file server in order to DELETE , UPLOAD , CREATE , MOVE files on a web server.
-> It also facilitates authentication , we need USER/PASS to get into it.

# WebDAV Exploitation
1. First , identify whether it is configure to MS IIS or NOT
	-> use nmap's `http-enum.nse` script
2. if YES , then Brute-Force the credentials in order to get into the server
3. After that , upload a malicious `.asp` file , used to execute arbitrary commands and get a Reverse Shell.
#webdav #DAV
###### Tools
1. `davtest` : used to scan , auth & exploit a DAV server.
	-> `davest -url http://IP/webdav` 
	-> give info about which filetypes can be upload , is directory writeable or not etc.

2. `cadaver` : It supports , file uploads , download , on screen display , in place editing , name-space operation (move/copy) , collection creation & deletion , property manipulation & resource locking on DAV servers.
	-> `cadaver http://IP/webdav` : fill user and password in prompt and use `put`
	command to upload a file like `.asp` reverse shell.
	-> */usr/share/webshells/asp/webshell.asp*

--> _NOTE_  :  Use hydra to Brute Force WEBDAV user/pass.


# Exploiting SMB With PsExec
###### SMB #smb
-> Two levels of authentication :
	-> User auth : needed _username & password_
	-> Share auth : needed _password_ only

-> they both are using , challenge-response based authentication

###### PsExec
-> Lightweight replacements of _telnet_ 
-> allows us to executes command & processes on a remote computer using any user's  credentials
-> authentication is performed via *SMB*
-> Just like RDP but it sends commands via CMD not in GUI like RDP.

###### Brute Force
-> But we need USER & PASSWORD to use _PsExec_
-> Best way to get it by _Brute Force_ 
-> narrow down the attack by just adding common user names like : _administrator_
-> after getting a valid user/pass , we can execute commands and get reverse shells.

###### commands 
`psexec.py USER@IP cmd.exe`
 -> enter password in prompt.

another then PS exec : `/exploit/linux/samba/is_known_pipename`
-> MSF Module used to exploit pipename.

# Exploiting RDP
1. Firstly, get to know which port it is running ! 
-> rdp_scanner MSF mdoule
-> manually connect to that port 
#rdp
2. Burteforcing RDP
-> *Hydra*
-> `hydra -L users.txt -P passwords.txt rdp://IP -s 3333`

3. connection
	-> `xfreerdp /u:USERNAME /p:PASSWORD /v:IP:PORT`
	-> Enjoy your GUI window.

# Exploiting WinRM 
-> windows remote management protocol
-> _5985 / 5986_ PORTs 
#winrm
###### Tools
-> _crackmapexec_ : to perform a brute-force
`crackmapexec winrm IP -u administrator -p /usr/pass_list.txt`
`crackmapexec winrm IP -u administrator -p PASSWORD -x "COMMAND"`

-> *evil_winrm* : ruby scrip to obtain a command shell.
`evil-winrm.rb -u USERNAME -p PASSWORD -i IP` : enjoy your shell.

##### steps to hack with MSF
1. first , check out the authentication methods supported by the  winRM on the target.
	-> `/auxiliary/scanner/winrm/winrm_auth_methods`
2. If , _basic auth method_ is there then we can brute force it 
	-> `/auxiliary/scanner/winrm/winrm_login`
3. After getting username and password , we can execute the commands 
	-> `/auxiliary/scanner/winrm/winrm_cmd`
4. Module which provide _REMOTE CODE EXECUTION_
	-> `/auxiliary/scanner/winrm/winrm_script_exec`
	-> Don't forgot to `set FORCE_VBS TRUE` , set it on true



