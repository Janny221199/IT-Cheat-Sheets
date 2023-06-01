
# Username Enumeration
creating a list of valid usernames
a lot of websites return a Error message / HTTP Status which indicates that the username / email exists. e.g.:
- wrong password in login (and for other random (non existing) usernames there is another error message)
- username / emal already exists in registration

for emails you should use [Social Enigineering](../Social%20Enigineering.md) or [OSINT](OSINT.md)

for usernames:
Adjust Http method and fieldnames and error message and so on
`-mr` is the text on the page we are looking for to validate we've found a valid username. (match regular expression)
![](attachments/Pasted%20image%2020230602000640.png)
you can also specify specific http status codes with `-mc` 
```
ffuf -w WORDLIST -X POST -d "username=FUZZ&email=x&password=x&cpassword=x" -H "Content-Type: application/x-www-form-urlencoded" -u URL/customers/signup -mr "username already exists"
```


# Brute force
create a file with valid usernames / emails from [Username Enumeration](Authetication%20Bypass.md#Username%20Enumeration) e.g. `valid_usernames.txt`

`W1` = list of valid usernames
`W2` = list of possible passwords
`-fc` = check for an HTTP status code other than 200
```
ffuf -w valid_usernames.txt:W1,:W2 -X POST -d "username=W1&password=W2" -H "Content-Type: application/x-www-form-urlencoded" -u URL/customers/login -fc 200
```


# Logic Flaws
search for logic flaws in webiste
e.g.:
- PHP Backend validates if the user role of a user visiting an admin page only if the URL starts with `/admin`, but makes it case sensetive and you can change the request to a different case like `/adMin` 
```php
if( url.substr(0,6) === '/admin') {
    # Code to check user is an admin
} else {
    # View Page
}
```
- API password reset takes a username of the account and email to send the confimation link separately and does not check if they belong together or so on..