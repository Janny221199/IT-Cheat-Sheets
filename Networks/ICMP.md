# ICMP
- **I**nternet **C**ontrol **M**essage **P**rotocol
- send error messages and operational information indicating success or failure when communicating with another [IP-Address](IP-Address.md)
- typical ICMP messages:
	- `NETWORK UNREACHABLE`:  Packet could not be delivered
	- `TIME EXCEEDED`: TimeToLive (TTL) is expired (see TTL in IP [Packets](OSI-Modell/3-Network-Layer.md#Packets))
	- `ECHO REQUEST/REPLY`: host still alive? (ping)
- ICMP messages are stored in the Data of an IP [Packets](OSI-Modell/3-Network-Layer.md#Packets)
- ICMPv6 for [IPv6](IP-Address.md#IPv6)