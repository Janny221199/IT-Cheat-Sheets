- getting a shell locally which executes commands remotely ([Remote Code Execution RCE - Command Injection](Remote%20Code%20Execution%20RCE%20-%20Command%20Injection.md)) on the vulnerable server remotely to access the computer

# Deploying a Reverse Shell
1. locally listen on a port for a connection from the vulnerable computer: `nv -lnvp PORT`
2. initiate the connection from the vulnerable computer
	1. deploying a script which connects to your local PC. for example for php: https://raw.githubusercontent.com/pentestmonkey/php-reverse-shell/master/php-reverse-shell.php and change the IP address to your computer listening via nc
	2. one way to uploads the script is via a vulnerable file upload like [Deploying a RCE Webshell via File Upload](Remote%20Code%20Execution%20RCE%20-%20Command%20Injection.md#Deploying%20a%20RCE%20Webshell%20via%20File%20Upload) and activate the script by accessing the file in the browser