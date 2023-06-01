
find hidden Directories and Files of a website
using wordlists [Wordlists](Kali%20Ressources%20and%20Files.md#Wordlists)
-> also see [Robots.txt and sitemap.xml](OSINT.md#Robots.txt%20and%20sitemap.xml)

# Tools
## GoBuster Dir
`gobuster -u DOMAIN -w WORDLIST -o dirs.txt dir`
gobuster can also enumerate subdomains. see: [GoBuster DNS](Subdomain%20Enumeration.md#GoBuster%20DNS)

## Ffuf
`ffuf -w WORDLIST -u DOMAIN/FUZZ` FUZZ is the placeholder for the words of the wordlist

## dirb 
`dirb DOMAIN WORDLIST`