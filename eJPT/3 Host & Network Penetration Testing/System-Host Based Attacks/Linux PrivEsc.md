
#### Linux Kernel Exploitation
`pre-requisites` :
	-> Identifying Kernel version
	-> Identifying Kernel vulnerabilites
	-> downloading , compiling & transfering exploits to the target machine

`tools` :
	-> `linux exploit suggester` 
		-> run this on target , get the exploit from the URL provided
		-> download it on ATTACKER machine.
		-> Compile it and tranfer the executable on TARGET
		-> execute , if it is executing , then file 
		-> otherwise , compile the `.c` file on TARGET itself and then execute.
		-> it may run fine , if the target is vulnerable


#### Misconfigured Cron Jobs
-> listing cron jobs
	-> `crontab -l`


1. Find a script associated with the `root` account , which looks sus
2. try to find that script path , if it might get used in any script
	-> `grep -rnw / -e '/FILE/PATH/TO/SEARCH/FOR'`
	-> `grep -rnw / -e '/home/kali/script`
3. If , it gets used in any script , that script will come , check the perms associated with that script , if you could change it , it might have a good vector for `cron job privesc`

4. change the file content accordingly , like modification in `sudoers` file.
	-> `printf '#!/bin/bash\n echo "kali ALL=NOPASSWD:ALL" '  >> /etc/sudoers > /PATH/TO/SCRIPT` 


#### Exploiting SUID Binaries
`pre-requisites` :
	-> owner of the file must be a `root` user.
	-> files must have SUID bit set 
	-> Executable perms must be allocated.

-> Scenerios

1. `file filename` : look on the _shared objects_ , in case if it's _missing_ , make you own malicious shared object on same path with same name and get it executed.

2. `strings filename` : look for any external script is being used in the file , if you can edit that external script then , take a shell accrodingly.

etc.

#### Weak File Perms
1. Find the files with weak perms
	-> `find / -not -type l -perm -o+w`
	-> command to search files / dirs which are having writable perms for everyone


