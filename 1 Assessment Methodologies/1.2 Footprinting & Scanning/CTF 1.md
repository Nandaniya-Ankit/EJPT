# Flag 1
The server proudly announces its identity in every response. Look closely; you might find something unusual. 
- Capture the response on the site in burpsuite.

# Flag 2
The gatekeeper's instructions often reveal what should remain unseen. Don't forget to read between the lines.
- Check robots.txt
- then go to `/secret-info/`
- under that `/secret-info/flag.txt`
# Flag 3
Anonymous access sometimes leads to forgotten treasures. Connect and explore the directory; you might stumble upon something valuable.
Login with FTP Anonymous
```
ftp demo.ine.local
	name: anonymous
	password:
get flag.txt creds.txt
cat flag.txt creds.txt
```
# Flag 4
A well-named database can be quite revealing. Peek at the configurations to discover the hidden treasure.
- Login with the found credentials
```
mysql -h target.ine.local -u db_admin -p
	password@123
show DATABASES;
```