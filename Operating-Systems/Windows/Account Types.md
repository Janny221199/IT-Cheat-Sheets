- profile for user in `C:\Users`
- each user profile will have the same folder e.g.:
	-  Desktop
	-  Documents
	-  Downloads
	-  Music
	   Pictures
- display users and groups: `lusrmgr.msc`

# Standard User
- A Standard User can only make changes to folders/files attributed to the user & can't perform system-level changes, such as install programs

# Administrator
- An Administrator can make changes to the system: add users, delete users, modify groups, modify settings on the system, etc.


# User Account Control UAC
- doesn't apply for the built-in local administrator account.
- When a user with an account type of administrator logs into a system, the current session doesn't run with elevated permissions. When an operation requiring higher-level privileges needs to execute, the user will be prompted to confirm if they permit the operation to run.