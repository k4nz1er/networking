- - -
protocol: ARP

layer: 2/3 (Data Link / Network)

RFC 826

source: "Andrew S. Tanenbaum, Computer Networks (4th Edition), pp. 515-516"
- - -

# ARP (Address Resolution Protocol)

Network layer software communicates using logical addresses ([[IPv4]]), but physical network hardware (such as Ethernet network interface cards) only recognizes physical Layer 2 Media Access Control (MAC) addresses. **ARP** bridges this boundary by dynamically resolving an unknown 48-bit MAC address corresponding to a known 32-bit IP address on the local broadcast domain.

- - - 

## Frame Layout & Protocol Fields

ARP packets are encapsulated directly inside Ethernet II frames without an IP or transport layer header (EtherType is set to `0x0806`).

```text
0                   1                   2                   3  
0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|         Hardware Type         |        Protocol Type        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
| Hardware Size | Protocol Size |            Opcode           |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                   Sender MAC (bytes 0-3)                    |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|     Sender MAC (bytes 4-5)    |    Sender IP (bytes 0-1)    |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|     Sender IP (bytes 2-3)     |    Target MAC (bytes 0-1)   |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                   Target MAC (bytes 2-5)                    |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                   Target IP (bytes 0-3)                     |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

|       Field Name       |  Size   | Purpose & Typical Value                                                  |
| :--------------------: | :-----: | ------------------------------------------------------------------------ |
| Hardware Type (HTYPE)  | 16 bits | Specifies the pysical layer network type (`1` for Ethernet)              |
| Protocol  Type (PTYPE) | 16 bits | Specifies the logical protocol being resolved (`0x0800` for IPv4)        |
| Hardware Length (HLEN) | 8 bits  | Length of physical address in octets (`6` for 48-bit MAC)                |
| Protocol Length (PLEN) | 8 bits  | Length of logical address in octets (`4` for 32-bit IPv4)                |
|         Opcode         | 16 bits | Type of ARP operation:<br>`1` for Request, `2` for Reply                 |
|   Sender MAC Address   | 48 bits | Layer 2 MAC address of the device sending the packet                     |
|   Sender IP Address    | 32 bits | Logical IPv4 address of the device sending the packet                    |
|   Target MAC Address   | 48 bits | Target MAC (`00:00:00:00:00:00` in requests; destination MAC in replies) |
|   Target IP Address    | 32 bits | Logical IPv4 address being queried                                       |
- - -

## Core Mechanics

Resolution Workflow
1. **Local Delivery:** when Host 1 wants to send a datagram to Host 2 on the same subnet, it creates a broadcast frame querying: "Who owns IP `192.31.65.5`? Tell `192.31.65.7`"
2. **Layer 2 Broadcast:** the frame uses destination MAC `ff:ff:ff:ff:ff:ff`, requiring every NIC on the collision/broadcast domain to copy and parse it
3. **Unicast Reply:** only Host 2 responds directly to Host 1's MAC address with an ARP Reply: "I have `192.31.65.5`, my MAC is `E2`"
4. **Inter-Network Delivery:** routers do not forward Layer 2 broadcasts. If the destination is outside the local subnet, the host consults its routing table and sends an ARP query for the **default gateway's** IP address, encapsulating the outer frame with the router's ingress MAC



- - -
