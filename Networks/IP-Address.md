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
- 127.0.0.0/8 is the loopback address
- Problem: Scarcity of available IP addresses despite CIDR and subnets [Subnetting](Subnetting.md)


## IPv6
- set of [Base 16 Hexadecimal](../Computer-Science-Basics/Number-Systems.md#Base%2016%20Hexadecimal) characters
- 128 Bits (16\*8 Bits)
- IP Packet Header ([Packets](OSI-Modell/3-Network-Layer.md#Packets)) is not containing a checksum or fragmentation information like the [IPv4](IP-Address.md#IPv4) Header

## Private IP Address
- private address is used to identify a device amongst other devices
- use their private IP addresses to communicate with each other in one network
- not every single device needs a [Public IP Address](IP-Address.md#Public%20IP%20Address) public IP address (limit of IPv4 addresses) 

## Public IP Address
- public address is used to identify the device on the Internet
- multiple devices within on networks will use the same public IP address for data sent to the Internet
- given by an ISP (**I**nternet **S**ervice **P**rovider) like Telekom 



## Classful addressing
- [IPv4](IP-Address.md#IPv4) adress space divided in classes  A, B, C (D, E)
```
  0               8               16              24              31 = 32Bits = 4Bytes
  │ │ │ │ │ │ │ │ │ │ │ │ │ │ │ │ │ │ │ │ │ │ │ │ │ │ │ │ │ │ │ │ │
  └─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┘

  ┌─┬─────────────┬───────────────────────────────────────────────┐ 1.0.0.0
A │0│ Network     │ Host                                          │ to
  └─┴─────────────┴───────────────────────────────────────────────┘ 127.255.255.255

  ┌───┬───────────────────────────┬───────────────────────────────┐ 128.0.0.0
B │1 0│ Network                   │ Host                          │ to
  └───┴───────────────────────────┴───────────────────────────────┘ 191.255.255.255

  ┌─────┬─────────────────────────────────────────┬───────────────┐ 192.0.0.0
C │1 1 0│ Network                                 │ Host          │ to
  └─────┴─────────────────────────────────────────┴───────────────┘ 223.255.255.255

  ┌───────┬───────────────────────────────────────────────────────┐ 224.0.0.0
D │1 1 1 0│ Multicast address                                     │ to
  └───────┴───────────────────────────────────────────────────────┘ 239.255.255.255

  ┌───────┬───────────────────────────────────────────────────────┐ 240.0.0.0
E │1 1 1 1│ Reserved for future usage                             │ to
  └───────┴───────────────────────────────────────────────────────┘ 255.255.255.255 
```
- first leading bits of the address are reserverd by the class. 
	- e.g. every class B address starts with the first two bits 10
	 --> this limits the address range from 128.0.0.0 - 191.255.255.255
- fix amount of network adresses and host addresses in each class (see [Subnetting](Subnetting.md))
	- class A has less network addresses but a lot of host addresses per network -> good for great copmanies with a lot of hosts
	- class C has more network addresses but less host adresses -> better for private housings
- reserved private IP-Addresses (not from accessible from "internet"):
	- class A: 10.0.0.0/8
	- class B: 172.16.0.0/12
	- class C: 172.16.0.0/12
- Problem: the classdul addressing is classful addressing is not sparing with IP addresses (better way: [Subnetting](Subnetting.md) (CIDR))





