# ARP
- **A**ddress **R**esolution **P**rotocol
- allows a device to associate its [MAC Address](MAC-Address.md) with an [IP Address](IP-Address.md) on the network
- Each device on a network will keep a log of the MAC addresses associated with other devices

1. ARP-Request is sent
2. message is broadcasted to every network device
3. the device which MAC Address matches the requested IP Address 
4. requested device sends an ARP-Reply as acknowledgement
5. other device will now store an ARP-Entry 