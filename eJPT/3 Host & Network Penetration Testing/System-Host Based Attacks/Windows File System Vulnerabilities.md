
#### Alternate Data Streams
-> It is basically a file attribute of NTFS , designed to provide compatibility to 
	MacOS Hierarchical File System HFS

-> file created on NTFS , have two forks/Streams
1. Data Stream : where the data is being stored.
2. Resource Stream : Metadata of the file.

-> ADS can be used to hide malicious code for execution to evade AVs & scanner tools

###### steps
-> TO HIDE A `.TXT` FILE
	-> `notepad rghx.txt`
	-> `notepad rghx.txt:secret.txt`
	-> if you want to read the file `secret.txt`
	-> `notepad rghx.txt:secret.txt`

-> TO HIDE A `.exe` FILE , the process is almost SAME

-> let's say we want to run a `payload.exe` whenever a `notes.txt` is opened.
-> then , first hide the `payload.exe` in `notes.exe`
-> `type payload.exe > notes.txt:payload.exe`
-> `start notes.txt:patload.exe`  -> to run payload
-> if you want to create a command to run this ,
-> then create a symbolic link ,  
	-> go to `C:\Windows\system32`
	-> `mklink wupdate.exe C:\Temp\notes.txt:payload.exe`
	-> now whenever you try to type the command `wupdate` it will run the `payload.exe`

