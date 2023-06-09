- allows attackers to gain complete control over an affected web site and the underlying web server
- execute commands on the operating system using the same privileges that the application on a device is running with (same user)
- command injection vulnerabilities arise when an application incorporates user data into an operating system command that it executes. An attacker can manipulate the data to cause their own commands to run.
- vulnerability exists because applications often use functions in programming languages such as PHP, Python and NodeJS to pass data to and to make system calls on the machine’s operating system e.g. searching for an entry in a file
- execute any commands or spawn a reverse shell using nc (TODO link reverse shell)

often server takes some value and appends it to a command, e.g.:
Server takes URL as user input and appends it to `ping ` -> `ping URL` -> you can use the `&&` operator to execute a different command with the input `URL && whoami`


# Blind RCE
- no direct output from the application when testing payloads
- investigate the behaviours of the application to determine whether or not your payload was successful
- use payloads that will cause some time delay e.g. `ping` or `sleep` -> the application will hang for _x_ seconds in relation to how many pings specified


# Verbose RCE
- direct feedback from the application once you have tested a payload
- For example, running the `whoami` command to see what user the application is running under. The web application will output the username on the page directly


# Remediation: How to Prevent Command Injection
- avoid incorporating user-controllable data into operating system commands
- user data should be strictly validated, ideally with a whitelist of specific values
- reject whitespaces