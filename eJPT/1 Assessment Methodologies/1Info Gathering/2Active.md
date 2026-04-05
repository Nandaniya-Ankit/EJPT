### DNS Zone Transfer
#zone-interrogation #zone-transfer 

#### DNS Records
`A` - Resolves a hostname or domain to an IPv4 address.
`AAAA` - Resolves a hostname or domain to an IPv6 address.
`NS` - Reference to the domains nameserver.
`MX` - Resolves a domain to a mail server.
`CNAME` - Used for domain aliases.
`TXT` - Text record.
`HINFO` - Host information.
`SOA` - Domain authority.
`SRV` - Service records.
`PTR` - Resolves an IP address to a hostname

==Host File:  /etc/hosts==

##### Tools : 
1. `dnsrecon`  : `dnsrecon -d <web url>`
2. `dnsenum` : 
```
dnsenum web_url
```
3. `dig` : 
```
dig axfr @name-server domain               | axfr= zone transfer switch
dig axfr ansztml.digi.ninja zonetransfer.me
```
4. `fierce` : 
```
fierce -dns DOMAIN.com
```
#### Host Discovery 

###### nmap
1. `-sn` : flag to deny port scan and scan host/IPs on the target
	-> `sudo nmap -sn IP`
###### netdiscover (scan hosts on LOCAL network)
1.  `netdiscover` : tool which is used to discover host on network 
	-> it also captures ARP request to get MAC to IP & IP to Mac.
		-> `netdiscover -i INTERFACE` 
		->`netdiscover -r Range` *| 192.168.1.0/24 OR 192.168.1.10-50 |*
		-> `netdiscover -n 20` -> will only scan first 20 hosts.
		-> `netdiscover -l` -> list available network interface.
		-> `netdiscover -i INTERFACE -r network/subnet` 
```
sudo netdiscover -i etho0 -r 192.168.1.0/24
```
#### Port Scanning

1. `nmap -Pn -F -sU -sC -sV -O -v -T 5 IP -oN test.txt`
2. `-F`: Fast scan | will scan most common 1000 ports
3. `-sU`: default scan will only scan TCP ports, scan UDP ports
4. `-sV`: Service version detection scan
5. `-sC`: Default script scan
6. `-O`: Operating system detection
7. `-Pn`: Disables the ping/host discovery phase
8. `-v`: Shows more detailed output during the scan
9. `-A`: Aggressive Scan which include `-sV`, `-O`, `-sC`
10. `-T`: Set timing templet *-T 0 to 5*
11. `-oN` `oX` :Output the result in a file 
	1. test.txt | same CLI output in txt file
	2. test.xml