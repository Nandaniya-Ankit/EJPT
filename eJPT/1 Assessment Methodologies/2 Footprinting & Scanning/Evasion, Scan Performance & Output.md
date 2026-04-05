## Firewall Detection & IDS Evasion:

```
nmap -Pn -sA -p445,3389 10.4.27.83
nmap -Pn -sS -sV -p 80,445,3389 -f 10.4.27.83
nmap -Pn -sS -sV -p 80,445,3389 -f --mtu 8 10.4.27.83
nmap -Pn -sS -sV -p 80,445,3389 --data-length 200 -D 10.10.23.1,10.10.23.2 10.4.27.80

nmap -Pn -sS -sV -p 80,445,3389 --data-length 200 -g 53 -D 10.10.23.1,10.10.23.2 10.4.27.80

```

1. `-sA` To detect the presence of a firewall or a filtering mechanism.
	ACK flag is used to send ACK set packet to target and tells the STATE , whether FILTERED (_FW is there_)or UNFILTERED (_FW is NOT there_).
2. `-f`  flag is used for **packet fragmentation**, a technique primarily employed for firewall and IDS evasion. 
	-> `MTU`: *Maximum Transmitted Unit*
		used to set the custom value with `-f` flag.
3. `--data-length`: Append random data to sent packets
4. `-D` Decoy IPs address/Spoofing | like giving the Gateway IP and other separated by ","
		then the target IP with space in between.
	|->`-g`:Used to specify a particular **source port** for the outgoing scan packets.
5. `--ttl`: Sets the IPv4 time-to-live field in sent packets to the given value.

---
## Optimizing Nmap Scans:

It is primarily used to avoid triggering rate-limiting mechanisms on firewalls, IDS, or target operating systems that block high-frequency scanning activity.

```
nmap -sV -sS -F --host-timeout 5s 10.10.23.0/24
nmap -sV -sS -F --scan-delay 15s 10.4.26.5
nmap -sV -sS -F -T1 10.4.26.5
```

1. `--host-timeout`: Give up on target after this long/specified time.
2. `--scan-delay`: a specified time interval (e.g., `1s`, `500ms`) between probe packets sent to a target host. 
3. `-T`: Used for setting **timing templates** to control the speed and aggressiveness of a scan.
	- `-T0` *(Paranoid):* Extremely slow, used to avoid triggering IDS.
	- `-T1` *(Sneaky):* Very slow, also aimed at IDS evasion.
	- `-T2` *(Polite):* Slower than default to reduce bandwidth usage and load on the target.
	- `-T3` *(Normal):* The default behaviour, dynamic timing based on network responsiveness.
	- `-T4` *(Aggressive):* Recommended for stable, fast networks. Significantly faster than default, but might overwhelm targets.
	- `-T5` *(Insane):* Very aggressive, sacrifices accuracy for speed; likely to miss open ports or crash services.

--> `--script-args` : used to pass arguments to the script like here _smbusername_ & _smbpassword_.

---
## Nmap Output Formats:

```
nmap -Pn -sA -p- 10.4.27.83 -oN nmap_simple.txt
nmap -Pn -sA -p- 10.4.27.83 -oN nmap_xml.xml
nmap -Pn -sA -p- 10.4.27.83 -oN nmap_grep.txt
```
