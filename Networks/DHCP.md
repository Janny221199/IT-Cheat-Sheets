# DHCP
- **D**ynamic **H**ost **C**onfiguration **P**rotocol
- assign an IP Address to a new (not manually assigned) network device


```
New Network Device               DHCP SERVER
   │                                    │
   │                                    │
   │           DHCP Discover            │
   ├───────────────────────────────────►│
   │                                    │
   │                                    │
   │                                    │
   │                                    │
   │           DHCP Offer               │
   │◄───────────────────────────────────┤
   │                                    │
   │                                    │
   │                                    │
   │                                    │
   │           DHCP Request             │
   ├───────────────────────────────────►│
   │                                    │
   │                                    │
   │                                    │
   │                                    │
   │           DHCP ACK                 │
   │◄───────────────────────────────────┤
   │                                    │
   │                                    │
   │                                    │
```

Method | Description
--- | ---
DHCP Discover | Device checks if any DHCP servers are on the network
DHCP Offer | DHCP server replies with an IP address the device could use
DHCP Request | Device replies and confirms that it will use this address for the IP Address
DHCP Ack | DHCP server replies with an acknowledgment that the device can start using the IP Address