- Insecure Direct Object Reference
- type of (broken) access control vulnerability
-  If a website visitor can access protected pages they are not meant to see, then the access controls are broken.
- view sensitive data
- Accessing unauthorized functionality
- arises when an application uses user-supplied input to access objects directly with too much trust in the input data without validating if the requested data belongs to the user data

Example:
`http://URL/account?id=111` -> my account

try out another id to view that account ->  `http://URL/account?id=222`


- used in post data, query strings, cookies and so on
- data could be encoded / hashed (so must be decoded, changed and encoded again to request other objects)
- best way to check it with 2 accounts
	- have a look at the Web Browser Developer Network Tab, find possible Request, edit values and request again
