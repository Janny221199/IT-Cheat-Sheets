# IP Address
- **I**nternet **P**rotocol
- identifying a host on a network for a period of time
- [IPv4](IP-Address.md#IPv4) and [IPv6](IP-Address.md#IPv6)
- set of numbers (and characters in case of IPv6)
-  calculated through a technique known as IP addressing & [Subnetting](Subnetting.md)
-  cannot be more than once within the **same** network
-  public and private IP Address
- most important protcol for the [3-Network-Layer](OSI-Modell/3-Network-Layer.md)
- hierarchical
- contains a netword id and host id seperated with a network mask / subnet mask

## IPv4
- **192.168.2.117**
- set of numbers
- separated by '**.**' in 4 octets
- 32 Bits (4\*8 Bits) -> 4 \* numbers between 0 and 255
- 2^32 -> 4.29 billion IPv4 addresses (problem)
- separating networks in a [Classful Design](IP-Address.md#Classful)
- after the network part (netmask [Subnetting](Subnetting.md)) all bits to 0 (network address) and all bits to 1 (broadcast address) are reservered adresses


## IPv6
TODO

## Private IP Address
- private address is used to identify a device amongst other devices
- use their private IP addresses to communicate with each other in one network
- not every single device needs a [Public IP Address](IP-Address.md#Public%20IP%20Address) public IP address (limit of IPv4 addresses) 

## Public IP Address
- public address is used to identify the device on the Internet
- multiple devices within on networks will use the same public IP address for data sent to the Internet
- given by an ISP (**I**nternet **S**ervice **P**rovider) like Telekom 



## Classful
TODO link to subnetting
TODO 192 or 127 missing for private or callback??
TODO A and B only a few networks but a lot of hosts
TODO C a lot of networks but not so much hosts





