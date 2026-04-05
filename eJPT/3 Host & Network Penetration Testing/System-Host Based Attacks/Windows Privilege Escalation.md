###### automation
-> _PrivescCheck_ : Github tool to automate check for priv esc vectors
	-> https://github.com/itm4n/PrivescCheck 
	
----
###### psexec (SMB Login)

-> In case of having a valid set of _Credentials_
-> use `psexec` to connect with the target machine and get shell
-> either through _MSF Module_ or _github binary_

---
-> `net localgroup GROUPNAME` : to see which users comes under this group 

#### Kernel Exploits
-> Windows NT is the default kernel in windows
###### Steps :
-> Identifying Kernel vulnerability (according to the version of window) 
-> Downloading , compiling & transferring the exploit on the target machines.

###### Tools 
1. Windows-Exploit-Suggester
	-> https://github.com/AonCyberLabs/Windows-Exploit-Suggester
2. Windows-Kernel-Exploits
	-> https://github.com/SecWiki/windows-kernel-exploits
	-> collection of windows kernel exploits sorted by CVE.

###### MSF
-> `getsystem` : meterpreter command which tried to get escalated privileges automatically by using some common PrivEsc vectors.

-> */multi/recon/local_exploit_suggester* : MSF Module to know which kernel expliot is usefull to execute 
	-> 

----
#### Bypassing UAC With UACMe
-> UAC : User Access Control.
###### Needs #important 
-> need to have a user account , that must be the part of a local administrator group
	`net localgroup GROUPNAME`
-> need to know the UAC level , high , medium or low.

###### tool
1. `UACMe` : https://github.com/hfiref0x/UACME

--> methodology 
1. generate own payload using MSFVENOM
2. Setup the /multi/handler listener on MSF
3. transfer the `akagai64.exe` & `paylaod` on windows machine.
4. run the `akagain64.exe` [ key ]  [ payload to execute ]
	-> `akagain64.exe 23 C:\\Temp\payload.exe`
	-> these keys are based on the windows version running on the machines and a proper description is given on  `github`.

###### using MSF

-> `/exploit/windows/local/bypassuac_injection`
-> `set PAYLOAD windows/x64/meterpreter/reverse_tcp`
-> `set TARGET Windows\ x64`  : tab can help in auto completion


----
#### Access Token Impersonation
-> access token are generate by `winlogon.exe`
###### needs
1. need to have an elevated account or a low privileged account that have an 
	-> _SeimpersonatePrivilege_ permission
2. A foothold on system

3. Impersonate-level token : created as a direct result of non-interactive login 
4. Delegate-level token : created through an interactive session like RDP etc.

###### Privs
1. SeAssignPrimaryToken : This allows user to impersonate token
2. SeCreateToken : This allows a user to create an arbitrary token with administrative privileges.
3. *SeImpersonatePrivilege* : This allows a user to create a process under the security context of another user typically with administrative privileges.

###### Tools
-> rejetto : MSF exploit module -- exploit for *HttpFileServer httpd 2.3*
-> `Incognito` : MSF plugin to impersonate tokens 

###### steps
1. use rejetto , to get do a command injection attack on `HttpFileServer` 
2. set options and exploit , get a meterpreter shell
3. check , if we have `SeImpersonatePrivilege` perms
4. after that , `load incognito` .
5. `list_tokens -u` : to list user accounts access tokens
6. copy the name of the access_token and run
7. `impersonate_token "NAME"`
8. migrate to a different service : `migrate PID` : might be explorer.exe

----
	