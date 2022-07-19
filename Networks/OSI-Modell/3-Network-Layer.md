# Network Layer
- Transmission of data between participating stations
- must not be connected physically
- finding the right way to a destination ([Forwarding](#Forwarding) / [Routing](#Routing))
- data transmission in packets (each packet is located in one or more frames of the [2-Data-Link-Layer](2-Data-Link-Layer.md))
- uses [IP-Address](../IP-Address.md)es as unique identifier for source and destination in local and world wide networks

## Forwarding
Forwarding refers to the router-local action of transferring packet from an input link interface to the appropriate output link interface.

## Routing
Routing refers to the network-wide process that determines the end-to-end paths that packets take from source to destination.

## Packets
```
 0               8               16              24              31 = 32Bits = 4Bytes
 │ │ │ │ │ │ │ │ │ │ │ │ │ │ │ │ │ │ │ │ │ │ │ │ │ │ │ │ │ │ │ │ │
 └─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┘

 ┌───────┬───────┬───────────────┬───────────────────────────────┐
 │Version│IHL    │Type of Service│Total Length                   │ 4Bytes─┐
 ├───────┴───────┴───────────────┼─────┬─────────────────────────┤        │
 │Identification                 │Flags│Fragment Offset          │ 4Bytes─┤
 ├───────────────┬───────────────┼─────┴─────────────────────────┤        │
 │Time to live   │Protocol       │Header Checksum                │ 4Bytes─┼─► 20Bytes
 ├───────────────┴───────────────┴───────────────────────────────┤        │
 │Source Address                                                 │ 4Bytes─┤
 ├───────────────────────────────────────────────────────────────┤        │
 │Destination Address                                            │ 4Bytes─┘
 ├───────────────────────────────────────────────────────────────┤
─┘                                                               └─
  Options                                                                   0-40Bytes
─┐                                                               ┌─
 ├───────────────────────────────────────────────────────────────┤
─┘                                                               └─
  Data                                                               Up to 65536Bytes
─┐                                                               ┌─
 └───────────────────────────────────────────────────────────────┘
```

- Version: IP-Protocol Version ([IPv4](../IP-Address.md#IPv4) OR [IPv6](../IP-Address.md#IPv6))
- IHL Internet Header Length: legnth of header in 32 Bit units (variable due to options and data)
- TOS Type of Service (also Differentiated Services Code Point (DSCP)):  shall distinguish packages with distinguish special requirements (e.g. multimedia)
- Total Length: total legth of the packet (max. 64KB)
- Identification, Flags and Offset:  allow fragmentation (TODO) of a packet by a router
- TTL Time to Live: amout of routers a package could be routed through (hop limit)
- Protocol: uesed Protocol in [4-Transport-Layer](4-Transport-Layer.md) (e.g. TCP / UDP)
- Header Checksum: calculated checksum of only IP-Header for error checking. Every Router is calculating the checksum and compares it to the checksum flied to check if the the packet is correct. if the values do not match, the router discards the packet.
- Source and Destination Address: [IP-Address](../IP-Address.md)es of source and destination 
- Options: not often used
- Data: Payload