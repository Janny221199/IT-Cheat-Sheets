TODO explain

# Favicon Discovery
Discover a used Framework if website did not change the frameworks favicon
1. get favicon: -> view source -> copy favicon url
2. get md5 checksum: `curl URL | md5sum`
3. get used frmework: find checksum in favicon database e.g.: https://wiki.owasp.org/index.php/OWASP_favicon_database
4. search for frameworks exploits