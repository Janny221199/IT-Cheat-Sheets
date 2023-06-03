- Domain Name System DNS turns dmain names into[IP-Address](IP-Address.md)es
- Domain Lookup: https://who.is/
- DNSv6 for [IPv6](IP-Address.md#IPv6)

# Domain Hierachy
```
                                                            ┌─────────┐
                                                            │         │
                                                            │    .    │                                          Root Domain
                                                            │         │
                                                            └────┬────┘
                                                                 │
                                             ┌───────────────────┼───────────────────┐
                                             │                   │                   │
                                        ┌────┴────┐         ┌────┴────┐         ┌────┴────┐
                                        │         │         │         │         │         │
                                        │  .com   │         │  .edu   │         │   .io   │                      Top-Level Domain
                                        │         │         │         │         │         │
                                        └────┬────┘         └────┬────┘         └────┬────┘
                                             │                   │                   │
                         ┌───────────────────┤                   │                   ├───────────────────┐
                         │                   │                   │                   │                   │
                    ┌────┴────┐         ┌────┴────┐         ┌────┴────┐         ┌────┴────┐         ┌────┴────┐
                    │         │         │         │         │         │         │         │         │         │
                    │ google  │         │microsoft│         │   mit   │         │  janny  │         │   ...   │  Second-Level Domain
                    │         │         │         │         │         │         │         │         │         │
                    └────┬────┘         └─────────┘         └─────────┘         └────┬────┘         └─────────┘
                         │                                                           │
     ┌───────────────────┤                                       ┌───────────────────┼───────────────────┐
     │                   │                                       │                   │                   │
┌────┴────┐         ┌────┴────┐                             ┌────┴────┐         ┌────┴────┐         ┌────┴────┐
│         │         │         │                             │         │         │         │         │         │
│  cloud  │         │  maps   │                             │  mail   │         │   app   │         │   www   │  Subdomains
│         │         │         │                             │         │         │         │         │         │
└─────────┘         └─────────┘                             └─────────┘         └─────────┘         └─────────┘
```

## Top-Level Domain TLD
- most righthand part of a domain name
- full List: https://data.iana.org/TLD/tlds-alpha-by-domain.txt
### gTLD
- historically gTLD was meant to tell the user the domain name's pupose:
	- .com commercial
	- .org organisation
	- .edu education
	- . gov government
- Due to such demand, there is an influx of new gTLDs ranging from .online , .club , .website , .biz and so many more
### ccTLD
- geographical purposes:
	- .us United States
	- .ca Canada
	- .de Germany (Deutschland)

## Secon-Level Domain
- right before the TLD
- needs to be registered
- could be choosen by the buyer
- limited to 63 characters + TLD
- only `a-z` and `0-9` and hyphens `-`(but a hyphen cannot be at the start or end)

## Subdomain
- on the left-hand side of the Second-Level Domain using a period to separate it e.g. `mail.janny.io`
- same name limitations as [Secon-Level Domain](DNS.md#Secon-Level%20Domain)
- You can use multiple subdomains split with periods to create longer names, such as `abc.def.janny.io` But the length must be kept to 253 characters or less. 
- There is no limit to the number of subdomains you can create for your domain name.

# DNS Server
- A DNS server is a computer with a database containing the public IP addresses associated with the names of the websites an IP address brings a user to. DNS acts like a phonebook for the internet.

There ar 4 DNS Servers involved in translating a domain:

## DNS recursor
The recursor can be thought of as a librarian who is asked to go find a particular book somewhere in a library. The DNS recursor is a server designed to receive queries from client machines through applications such as web browsers. Typically the recursor is then responsible for making additional requests in order to satisfy the client’s DNS query.

## Root Nameserver
The root server is the first step in translating (resolving) human readable host names into IP addresses. It can be thought of like an index in a library that points to different racks of books - typically it serves as a reference to other more specific locations.

## TLD Namesever
The top level domain server (TLD) can be thought of as a specific rack of books in a library. This nameserver is the next step in the search for a specific IP address, and it hosts the last portion of a hostname (In example.com, the TLD server is “com”).

## Authoritative Nameserver
Authoritative nameserver - This final nameserver can be thought of as a dictionary on a rack of books, in which a specific name can be translated into its definition. The authoritative nameserver is the last stop in the nameserver query. If the authoritative name server has access to the requested record, it will return the IP address for the requested hostname back to the DNS Recursor (the librarian) that made the initial request.


# DNS Records
- most record types have a `TTL` field how long a DNS record should be cached

## A Record 
These records resolve to IPv4 addresses

## AAAA Record 
These records resolve to IPv6 addresses

## CNAME Record 
These records resolve to another domain name
Another DNS request would then be made to the reference domain name to work out the IP address.

## MX Record 
These records resolve to the address of the servers that handle the email for the domain you are querying
These records also come with a priority flag. This tells the client in which order to try the servers, this is perfect for if the main server goes down and email needs to be sent to a backup server.

## TXT Record
TXT records are free text fields where any text-based data can be stored. TXT records have multiple uses, but some common ones can be to list servers that have the authority to send an email on behalf of the domain (this can help in the battle against spam and spoofed email). They can also be used to verify ownership of the domain name when signing up for third party services.