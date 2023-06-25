TODO explain

- Open Web Application Security 
- free documentation, tools, videos, and forums

# OWASP Top 10 Vulnerabilities
regularly-updated report outlining security concerns for web application security, focusing on the 10 most critical risks

based on 2021:
1. Broken Access Control -> [IDOR](IDOR.md)
2. Cryptographic Failures -> using some unsecure encryption / hash methods -> [Crack Hashes](../Crack%20Hashes.md)
3. Injection -> [SQL Injection](SQL%20Injection.md) [Remote Code Execution RCE - Command Injection](Remote%20Code%20Execution%20RCE%20-%20Command%20Injection.md)
4. Insecure Design -> [Logic Flaws](Authetication%20Bypass.md#Logic%20Flaws)
5. Security Misconfiguration -> Poorly configured permission, having debugging or error message es displayed in production
6. Vulnerable and Outdated Components -> [CVEs](../CVEs.md)
7. Identification and Authentication Failures -> [Logic Flaws](Authetication%20Bypass.md#Logic%20Flaws)
8. Software and Data Integrity Failures -> [Subresource Integrity SRI](Subresource%20Integrity%20SRI.md)
9. Security Logging & Monitoring Failures -> [Save Logs](../Digital%20Forensics/Save%20Logs.md)
10. Server-Side Request Forgery (SSRF)

# Favicon Discovery
Discover a used Framework if website did not change the frameworks favicon
1. get favicon: -> view source -> copy favicon url
2. get md5 checksum: `curl URL | md5sum`
3. get used frmework: find checksum in favicon database e.g.: https://wiki.owasp.org/index.php/OWASP_favicon_database
4. search for frameworks exploits