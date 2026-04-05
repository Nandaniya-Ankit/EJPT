_NOTE_ :
	: while a *non-privileged* user , NMAP by defaults do a TCP Connect Scan.
		-> it is very _LOUD_ (does 3 way handshake) and not preferable 
		-> can only be useful if there's a consent for no detection on network & accuracy is required as it does 3 Way Handshake
	: While a *root* user : NMAP do a _SYN ACK_ scan

```
nmap -Pn -F 10.4.24.205
nmap -Pn -sS -F 10.4.24.205
```
`-F`: Fast profile | for scanning 1st 100 ports
_tip_ : 
	-> always use `-sS` SYN ACK Ping flag explicitly with a not privileged used.
	-> it is the most efficient scan because 
		-> SYN is not an UNCOMMMON packet so , it won't raise any alarm
		-> doesn't make any connection logs on target side as  not settuping full connection , send RST after getting SYN ACK
		->  SYN is NOT blocked also acceptable by almost every OS , firewall as it is necessary to do 3 way handshake.

#UDP : In case of not found anything open or usefull it's worth checking the UDP ports
	-> `nmap -sU IP`
	-> `nmap -sU IP -p 53,137,138,139`

#### TCP Connect Scan (full 3 way handshake)
```
nmap -Pn -sT 10.4.24.205
```
-> Default port scanning option used when nmap is run without root or sudo privileges. So in this particular scan, the TCP connects scan completes the three way handshake. 
The problem with this is because  it's very loud on a network and prone to detection by intrusion detection

#### UDP Scan
```
nmap -Pn -sU 10.4.24.205
```
##### Usage

1. Version Intensity of services :
	-> `nmap -p- -sV --version-intensity [0-9] IP -Pn -T4`
	-> higher the number , higher the possibility to a correct service & version.

2. OS guess (aggressively) :
	-> `nmap -p- -O --osscan-guess IP -Pn -T4`

#### Service Version & O.S. Version Detection

```
nmap -T4 -sS -sV --version-intensity 8 -O --osscan-guess -p- 192.31.214.3
```
##### Usage:
1. `-sV --version-intensity`
	-> The intensity level ranges from 0 to 9.
	->The higher the intensity level increases the possibility of correctness.
2. `-O --osscan-guess`
	-> To try and detect the operating system aggressively.
	-> And here it will try and list out the version of the kernel's not the distribution.
#### Nmap Scripting Engine (NSE)

==Port scanning, service version detection, vulnerability scanning, exploitation, brute forcing, etc.==
_Nmap Script dir_ : _/usr/share/nmap/scripts_
$ Is -al /usr/share/nmap/scripts/ I grep -e "any_keyword"

1. `nmap -sS -sV -sC -p- -T4 192.224.77.3`
	->`-sC`=Default Script Scan
2. `nmap --script-help=Script_Name`
	-> tells about the script status whether is a default script & if it is safe to use

3. `nmap --script=SCRIPT,SCRIPT,SCRIPT IP`
	-> manually running Multiple scripts.
	-> EG: *nmap -ss -sv --script=mongodb-info -p- -T4 192.224.77.3*

4. `nmap --script=ftp-* IP`
	-> will run all script start from _ftp-_


TIP : adding all these three together in a single `flag` , `-A`
	-> service & version scans
	-> OS Scan
	-> Common Scripts

# LAB Commands
```bash
nmap 192.118.107.3 -p- -T4 -sS -sV --version-intensity 8 -O --osscan-guess

nmap 192.118.107.3 -p- -T4 -sS -sV --version-intensity 8 -O --osscan-guess -sC

nmap 192.118.107.3 -p- -T4 -sS -A
```

