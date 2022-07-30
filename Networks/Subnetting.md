# Subnetting
- related to CIDR **C**lassless **I**nter-**D**omain **R**outing (oppiste of [Classful addressing](IP-Address.md#Classful%20addressing))

## Netmask
- divides [IP-Address](IP-Address.md)es into subnets and specifies the network's available hosts
- `<ip-address> = <network><subnetwork><host>`
- subnets are not visible outside the network
- netmask could be specified in 2 notations:
	- integer (Postfix): counted from left how many bits are part of the network part `<ip>/<postfix>`
		- /24
		- /22
	- dottet decimal: network part is specified with a 1 and the host part is specified with a 0 `<ip>,<dotted decimal subnet>`
		- the 1 bits are all on the left and the 0 bits are all on the right
		- converted to a dotted decimal notation
		- 11111111.11111111.11111111. -> 255.255.255.0
	- 134.73.17.5/22 is equivalent to 134.73.17.5, 255.255.252.0
		- the network id would be 134.73.16.0 because ([Base 2 Binary](../Computer-Science-Basics/Number-Systems.md#Base%202%20Binary)):
		- IP 134.73.17.5 -> **10001111.01001001.000100**01.00000101
		- take the first 22 bits and make all other to 0, because of the network address ([IP-Address](IP-Address.md)) -> **10001111.01001001.000100**00.00000000 -> 134.73.16.0