# General 
1. to know the IP of website host `<website url>`
-> */robots.txt*
-> */sitemap.xml*
		pagesitemap
		authorsitemap
		sitemap-index.xml
		page-sitemap.xml
		category-sitemap.xml
		post-sitemap.xml 
---
#### Foot-printing Tools / Plugins 
1. `Wappalyzer` -> built-in technologies of the web site
2. `Builtwith` - Paid extension
3. `whatweb` -> command line version of wappalyzer
4. `webhttrack` -> tool to download a whole site in a local dir recursively
5. `whois` -> provide information of a website domain also *NAME SERVERS*
6. `host DOMAIN-NAME` -> will give IP of that site 
7. `Netcraft` -> A *one man army* tool for all mention tools above
	**Key Information Obtained from Netcraft:**
	1. Technology Stack Identification: Identifies web server OS, server-side technologies, and client-side frameworks.
	2. Infrastructure & Hosting: IP address, net block owner, hosting country, and historical hosting changes.
	3. Domain & DNS Details: Domain registrar information and DNS admin contacts.
	4. Security & SSL/TLS: Certificate authority (CA), SSL/TLS validity, expiration dates, and signed certificate timestamps.
	5. Vulnerability Assessment: Detection of security weaknesses like missing SPF/DMARC/DKIM records (email spoofing risk) and vulnerability to hotlinking.
	6. Site Reputation & History: When the site was first seen, site title, and ownership changes.
	7. Threat Detection: Identifies typosquatting, lookalike domains, and compromised websites.
--------
## DNS Recon
#### Tools : 
##### *DNSRecon*
- **Enumeration of subdomains**: DNSRecon can be used to enumerate all subdomains of a target domain.
- **Retrieval of DNS records**: DNSRecon can be used to retrieve DNS records for a target domain, including `Mail Exchange`(MX), `Name Server` (NS), `Start of Authority` (SOA), `TXT`, and other types of records. (`A`: IPv4 | `AAAA`: IPv6)
- **Brute-forcing of subdomains**: DNSRecon can be used to brute-force subdomains of a target domain.
- **Zone transfers**: DNSRecon can be used to perform zone transfers on a target domain. 
- **Reverse DNS lookups**: DNSRecon can be used to perform reverse DNS lookups for discovering the IP addresses associated with a domain.
##### *dnsdumpster.com*
- provides comprehensive data, including subdomains, IP addresses, hosting providers, DNS records (A, MX, NS, TXT), and visual network maps, *nmap scans* etc.
-------
#### WAF Foot-printing 
1. `wafw00f` -> used to see the WAF behind the web application.
	1. `waf00f IP/DOMAIN -a` | -a = Find all WAFs which match the signatures, do not stop testing on the first one
---
#### Subdomain Enumeration
1. `sublist3r` -> amazing tool to *passively* get the subdomains
	1. in case of you request is getting blocked , use a *VPN*.
----
#### Google Dorks
resource :  https://www.exploit-db.com/google-hacking-database.

1. site:
```
site:ine.com
site:*.ine.com -> to get the subdomain
```
2. inurl:
```
inurl admin
site:ine.com inur:admin
site:*.ine.com inur:admin
inurl:auth_user_file.txt  -> passwd file
inurl:passwd.txt
inurl:wp-config.bak 
	|-> DB pass backups file which devs probablly forgot to delete.
```
3. intitle:
```
site:*.ine.com intitle:admin
intitle index of
index of -> get the directory listing of any website
```
4. filetype
```
site:*.ine.com filetype:pdf
```
5. cache:
```
cache:ine.com
```
6. Google Hacking Database
-----
#### Email / Passwords Harvesting 
##### Tool : `theHarvester`
finds *emails, names, subdomains, IPs, URLs,*
```
theHarvester -d INE -b duckduckgo,baidu,bing,yahoo,urlscan
theHarvester -d ine.com -b duckduckgo,baidu,bing,yahoo,urlscan
```
#### Leaked password databases
http://haveibeenpwned.com
-> harvest the emails and try to get the password from this site