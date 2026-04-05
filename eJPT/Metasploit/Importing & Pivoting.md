1. Importing Nmap Scans in MSF
	-> first , start the postregsql database 
	-> `service postgresql start`
	-> `msfdb init`
	-> `db_status` to check the DB is connected with MSF or not 
	-> start MSF , `msfconsole -q` , enter , `workspace`
	-> `workspace -a NAME` -> to create and use workspace
	-> `db_import /PATH/FILE.xml` -> to import nmap output results
	-> `hosts` ,`services` etc are some commands to check 
	-> `db_nmap < args> IP` will help in performing nmap scans through MSF and it will automatically stores results in DB 

#### Pivoting
-> Creating a route b/w External IP and IP on its internal network 
	-> `run autoroute -s internal-IP`
	-> this command should run in meterpreter shell of the machines foothold.
	-> this will create a route b/w the machine we got foothold on that meterpreter and the machine present on their Internal Network.
	-> then we can use auxiliary port scanning modules to scan ports internally.

