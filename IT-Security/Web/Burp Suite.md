- java tool
- core: captures and manipulates all the traffic between web server and attacker
- testing web and mobile application

# Burp Suite Editions
## Community

| Tool      | Description                                                                                                                                  |
| --------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| Proxy     | intercept and modify requests/responses when interacting with web applications                                                               |
| Repeater  | capture, modify, then resend the same request numerous times (e.g manual testing of [Blind SQL Injection](SQL%20Injection.md#Blind%20SQL%20Injection)) |
| Intruder  | spray an endpoint with requests e.g. for Brute Force Attacks or automated testing of [Blind SQL Injection](SQL%20Injection.md#Blind%20SQL%20Injection) (but community edition is limited)                                              |
| Decoder   | decode and encode payload prior sending it to the target                                                                                     |
| Comparer  | compare two pieces at either word or byte level                                                                                              |
| Sequencer | check if tokens or session cookie values are really randomly generated                                                                       |

## Professional
- automated vulnerability scanner
- no rate limited bruteforcer
- saving projects for future usage 
- built-in API to allow integration with other tools
- unrestricted access to add new extensions

## Enterprise 
- continuous and automated scanning


# Proxy
The Burp Proxy works by opening a web interface on `127.0.0.1:8080` (by default). As implied by the fact that this is a "proxy", we need to redirect all of our browser traffic through this port before we can start intercepting it with Burp. 
e.g. in FireFox we can use FoxyProxy:
![](attachments/Pasted%20image%2020230610045236.png)

## allow HTTPS
we need to got to http://burp/cert with proxy enabled, download the `cacert.der` and import it to our browsers certifications
https://portswigger.net/burp/documentation/desktop/external-browser-config/certificate/ca-cert-chrome-macos
IOS: https://portswigger.net/burp/documentation/desktop/mobile/config-ios-device

## use Proxy

### Intercept
- click button so `Intercept is on`
- with the Button `Forward` and `Drop`in the Burp Proxy Tab we can decide if the request / response should be forwarded to the browser or not

### Scope 
- add a URL to the scope to log and intercept only traffic from that URL:
	- In `Target` Tab -> right click on URL -> add to scope -> in popup set logging only for that scope
	- in `Proxy` Tab in `Options` Sub-Tab select -> Intercept Client RequestsSection -> select `And` `URL` `Is in target scope`
	  ![](attachments/Pasted%20image%2020230610053012.png)

### Sitemap
- in `Target` Tab Creates a sitemap as a tree structure while we visit the page which shows the whole folder structure and so on

### Encode Payload in Request
- if we intercept a request we can change the payload
- to quickly encode the value we can mark it in Burp Suite and use `CTRL+U` to encode it

# Repeater
1. capture a Request with proxy
2. send it to Repeater `CTRL+R`
3. adjust values and send the request to the server
4. adjust value again and send it again

# Intruder 
1. capture a request with proxy
2. send it to Intruder `CRTL+I`
3. mark / select values which should be automated and brute forced
4. select a range or source for each selects position in Payloads subtype (e.g. a WordList, Number range...)
5. start attack
6. have a look a results (maybe filter HTTP responses or length (success responses might have a different length))

## Bypass CSRF
If you are required to send another dynamic value each time (e.g. a CSRF Token which is send within the previous request) you have to set up a `Macro` and `Session Handling Rules` for that.

Both in `Project Option` -> `Sessions` sub-tab:

**Macro:**
1. Add Macro
2. Select Target Endpoint and Method
3. give it a name / description
4. save Macro

**Session Handling Rules**
1. Scope Tab:
	1. Tools Scope -> deselect everything except Intruder
	2. URL Scope -> select `Use suite scope` and ensure that [Scope](Burp%20Suite.md#Scope) is set
2. Details Tab:
	1. set a description
	2. Rule Actions -> Add
	3. select `Run as Macro`
	4. select created Macro (this macro will now overwrite all of the parameters in our Intruder requests before we send them, based from the response before)
	5. Update only the following parameters -> Edit -> Add name of the parameter -> Ok 
	6. Update only the following cookies -> Edit Add name of the cookie -> Ok
	7. Ok
	   ![](attachments/Pasted%20image%2020230612031425.png)