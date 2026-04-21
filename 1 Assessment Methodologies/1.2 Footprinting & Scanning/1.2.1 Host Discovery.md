#### Host Discovery Techniques
1. _Ping Sweeps_ : Send ICMP echo requests , don't work for windows hosts as windows tend to block Ping Probes
2. _ARP Scanning_ : Used to scan hosts on local network , used by `netdiscover` tool.
3. _TCP SYN Ping_ : Half open scan , send TCP SYN , and after getting SYN , send RST packet _faster and safer_ -> `evade firewalls`
4. _UDP Ping_ : Effective for the host who doesn't respond to ICMP or TCP Probes.
5. _TCP ACK_ : the host is alive if it gets TCP RST as response otherwise expects no response.
6. _SYN ACK Ping_ : If TCP RST get as response means , host is alive.

###### ping sweeps
-> `ping`
```shell
ping -c 4 google.com # linux
ping -n 5 google.com # windows
```
-> `fping`  -> upgradation to ping
``` shell
fping -a IP 2>/dev/null
fping -a -g IP/SUBNET 2>/dev/null
```
`-a` = show targets that are alive
`-g` = generate target list (when subnet)
`2>/dev/null` = for host that are not reachable, no output to print 

-> setting the TTL value to the ping packet
```bash
ping -t 5 192.168.1.1 # Windows 
ping -m 5 192.168.1.1 # Linux
```

#### Host Discovery with NMAP

```
   nmap -sn 10.10.22.0/24
   nmap -sn 10.10.22.0/24 --send-ip
   nmap -sn -iL targets.txt
#TCP Sync Ping
   nmap -sn -PS 192.168.2.3
   nmap -sn -PS1-100 192.168.2.3
#TCP ACK Ping
   nmap -sn -PA 192.168.2.3
   nmap -sn -PA1-100 192.168.2.3
#ICMP echo Ping
   nmap -sn -PE 192.168.2.3 --send-ip
```
1. `-sn` : flag used to disable port scanning and can do host scanning only.
2. `--send-ip` : without this , `-sn` will only send ARP packets  , but after including this one , it will send packets with all protocols : ICMP , ARP etc.
		-> it is good to use as even if the ping probes are blocked by any host other requests will check if host is up or NOT.
3. `nmap -sn -iL hosts.txt`
	1. hosts list to check if they are online of not
##### TCP SYN Ping
#important
4. `nmap -sn -PS IPADDR` 
	1. `-PS` : flag which send TCP SYN Ping to target host 
	-> if target send SYN + ACK packet then the host is UP & port is OPEN
	-> if target send RST Packet then the host is UP but port is CLOSED
	-> if no response then host is DOWN
	-> if by-default sends the SYN packet to port 80 but can be change the port
		`nmap -sn -PS443 IPADDR` or 
		`nmap -sn -PS1-1000 IPADDR`
		-> if any of the port responded then the HOST is UP !!!

#tip
-> whenever performing any host scanning , use `-sn` otherwise the nmap start scanning ports also which take hell lot of time ~~
-> use `-sn` along with that , you can use other flags like `-PS` etc.
##### TCP ACK Ping
_Not a reliable method , as ACK packets probably blocked by most of the firewalls specially in windows_
5. `nmap -sn -PA IPADDR` 
	1. It will send TCP ACK to target 
	2. as sending TCP ACK initially is not a valid 3 way handshake process , so to close the connection the target will send RST packer
	-> if target send RST Packet then the host is UP but port is CLOSED
	-> if no response then host is DOWN
	-> change port : `nmap -sn -PA 1-1000 IPADDR`
##### ICMP Echo Request
1. `nmap -sn -PE IPADDR --send-ip`
	1. command to send ICMP echo packets
## Methodology 

0. `nmap -sn -v -T4 IPADDR` 
1. `nmap -sn -PS[ports] IPADDR` 
```bash
nmap -sn -PS 21,22,25,80,443,3389,8080 10.20.123.24
```
3. `nmap -sn -PS[ports] -PU[ports] IPADDR`
```bash
nmap -sn -PS21,22,25,80,443,3389,8080 -PU137,138 10.20.123.24
#53,137,138 -> DNS & netBios Port running over UDP Scans
```


