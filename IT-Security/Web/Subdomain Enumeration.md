finding used subdomains for a domain

# SSL/TLS Certificates
when a CA (Certificate Authority) creates a SSL/TLS Certificate for a domain there will be publicly created logs for the domain name in the CT (Certificate Transprency)
-> the purpose of a CT is to stop malicious and accidentally made certificates from being used
sites like 
- http://crt.sh/
- https://ui.ctsearch.entrust.com/ui/ctsearchui
offer a searchable database of certificates that shows current and historical results
-> we can use them to find subdomains


# Search Engines
use [Google](OSINT.md#Google) OSINT to search an public sites with a subdomain
`-site:www.DOMAIN  site:*.DOMAIN`
contain results leading to the domain name domain.com but exclude any links to www.DOMAIN therefore, it shows us only subdomain names belonging to the DOMAIN


# Sublist3r
this is a tool which combines and **automates** all OSINT subdomain enumeration methods
it uses Google, Baidu, Yahoo, Bing, Ask, SSL Certificates etc...
Download: https://github.com/aboul3la/


# Bruteforce 
## DNS Bruteforce on public DNS
### GoBuster DNS
`gobuster -d DOMAIN -w WORDLIST -o subsomains.txt dns`

### Dnsrecon
`dnsrecon -t brt -d DOMAIN`

## DNS Bruteforce on Virtual Hosts
Some subdomains aren't always hosted in publically accessible DNS results, such as development versions of a web application or administration portals. Instead, the DNS record could be kept on a private DNS server or recorded on the developer's machines in their /etc/hosts file which maps domain names to IP addresses.
Because web servers can host multiple websites from one server when a website is requested from a client, the server knows which website the client wants from the **Host** header. We can utilise this host header by making changes to it and monitoring the response to see if we've discovered a new website.
1. use `ffuf -w WORDLIST -H "Host: FUZZ.DOMAIN" -u DOMAIN`
   FUZZ is the playceholder for thereplaced words of the wordlist
   The **-H** switch adds/edits a header
   Because the above command will always produce a valid result, we need to filter the output. We can do this by using the page size result with the **-fs** switch. Edit the below command replacing {size} with the most occurring size value from the previous result. (using the size which is displayed the most often)
   2. `ffuf -w WORDLIST -H "Host: FUZZ.DOMAIN" -u DOMAIN -fs SIZE`
   