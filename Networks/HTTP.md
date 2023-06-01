- HTTP(s) = HyperText Transfer Protocol (Secure)
- HTTPS is the secure version of HTTP. HTTPS data is encrypted using [TLS](TLS.md) so it not only stops people from seeing the data you are receiving and sending, but it also gives you assurances that you're talking to the correct web server and not something impersonating it.
- developed by Tim Berners-Lee 1989 - 1991
- HTTP is the set of rules used for communicating with web servers for the transmitting of webpage data, whether that is HTML, Images, Videos, etc.


# Requests and Responses
When we access a website, your browser will need to make requests to a web server for assets such as HTML, Images, and download the responses.

Example Request:
```http
GET / HTTP/1.1
Host: janny.io
User-Agent: Mozilla/5.0 Firefox/87.0
Referer: https://janny.io/
```

- Line 1: This request is sending the GET method, request the home page with path / and telling the web server we are using HTTP protocol version 1.1.
- Line 2: We tell the web server we want the website janny.io
- Line 3: We tell the web server we are using the Firefox version 87 Browser
- Line 4: We are telling the web server that the web page that referred us to this one is https://janny.io
- Line 5: HTTP requests always end with a blank line to inform the web server that the request has finished.

Example Resonse:
```http
HTTP/1.1 200 OK
Server: nginx/1.15.8
Date: Fri, 09 Apr 2021 13:34:03 GMT
Content-Type: text/html
Content-Length: 98

<html>
<head>
    <title>Janny</title>
</head>
<body>
    Welcome To janny.io
</body>
</html>
```

- Line 1: HTTP 1.1 is the version of the HTTP protocol the server is using and then followed by the HTTP Status Code in this case "200 Ok" which tells us the request has completed successfully.
- Line 2: This tells us the web server software and version number.
- Line 3: The current date, time and timezone of the web server.
- Line 4: The Content-Type header tells the client what sort of information is going to be sent, such as HTML, json, images, videos, pdf, XML.
- Line 5: Content-Length tells the client how long the response is, this way we can confirm no data is missing.
- Line 6: HTTP response contains a blank line to confirm the end of the HTTP response.
- Lines 7-14: The information that has been requested, in this instance the homepage.


# HTTP Methods
HTTP methods are a way for the client to show their intended action when making an HTTP request.

common Methods:
- GET: getting information of a web server
- POST: submitting data to the web server and potentially creating new records
- PUT: submitting data to a web server to update information
- DELETE: deleting information/records from a web server


# HTTP Status Code 
status code give information about you should handle the reponse (error, success, redirect etc..)

## Status Code Ranges
Range | Purpose 
--- | ---
100-199 | Information Response (no longer very common)
200-299 | Success
300-399 | Redirection
400-499 | Client Errors
500-599 | Server Errors

## Common Status Codes

list of all: https://developer.mozilla.org/en-US/docs/Web/HTTP/Status

- 200 OK
- 201 CREATED
- 301 PERMANENT REDIRECT
- 302 TEMPORARY REDIRECT
- 400 BAD REQUEST (wrong / missing data)
- 401 NOT AUTHENTICATED
- 403 FORBIDDEN 
- 404 PAGE NOT FOUND
- 405 METHOD NOT ALLOWED
- 500 INTERNAL SERVICE ERROR (server does not know how to handle request properly)
- 503 SERVICE UNAVAILABLE (down or maintanance)


# Headers
Headers are additional bits of data you can send to the web server when making requests

## Common Request Headers
﻿These are headers that are sent from the client (usually the browser) to the server.

- **Host:** Some web servers host multiple websites so by providing the host headers you can tell it which one you require, otherwise you'll just receive the default website for the server.  
- **User-Agent:** This is your browser software and version number, telling the web server your browser software helps it format the website properly for your browser and also some elements of HTML, JavaScript and CSS are only available in certain browsers.  
- **Content-Length:** When sending data to a web server such as in a form, the content length tells the web server how much data to expect in the web request. This way the server can ensure it isn't missing any data.
- **Accept-Encoding:** Tells the web server what types of compression methods the browser supports so the data can be made smaller for transmitting over the internet.
- **Cookie:** Data sent to the server to help remember your information.

## Common Response Headers
These are the headers that are returned to the client from the server after a request.

- **Set-Cookie:** Information to store which gets sent back to the web server on each request.  
- **Cache-Control:** How long to store the content of the response in the browser's cache before it requests it again.  
- **Content-Type:** This tells the client what type of data is being returned, i.e., HTML, JSON, CSS, JavaScript, Images, PDF, Video, etc. Using the content-type header the browser then knows how to process the data.  
- **Content-Encoding:** What method has been used to compress the data to make it smaller when sending it over the internet.


# Cookies
- small piece of data that is stored on your computer
- Cookies are saved when you receive a "Set-Cookie" header from a web server
- Then every further request you make, you'll send the cookie data back to the web server. Because HTTP is stateless (doesn't keep track of your previous requests), cookies can be used to remind the web server who you are, some personal settings for the website or whether you've been to the website before
- often used for web authentication (token), but not recommended or shopping / tracking data
-