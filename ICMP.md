- - -
protocol: ICMP
layer: 3 (Network)
RFC 792
source: "Andrew S. Tanenbaum, Computer Networks (4th Edition), pp. 515-516"
- - -
# ICMP (Internet Control Message Protocol)

**ICMP** - is an auxiliary Layer 3 protocol [[OSI Model]] encapsulated directly within [[IPv4]] datagrams (assigned protocol number `1` in the IPv4 header). It does not carry application payload; instead, routers and destination hosts use it to report delivery errors, manage operational diagnostic telemetry, and signal unexpected transit failures.

- - -

## ICMP Header format

Every ICMP message starts with a mandatory 4-byte header followed by content specific to each message type:
```text
0                   1                   2                   3  
0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|     Type      |     Code      |          Checksum           |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|            Rest of Header / Identifier & Srquence           |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|  Original IPv4 Header + First 64 bits of Original Payload   |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

* **Type (8 bits):** defines the general classification of the ICMP message (e.g. Destination Unreachable, Echo Request);
* **Code (8 bits):** provides diagnostic detail specific to the message Type (e.g. Network unreachable vs Host unreachable vs Port unreachable);
* **Checksum (16 bits):** internet checksum calculated over the ICMP message (header + payload);
* **Diagnostic Data:** for error reports, ICMP includes the entire IPv4 header of the faulted packet plus its first 8 bytes (64 bits) of original payload, allowing the source to identify which socket and layer 4 port triggered the failure.

- - -

## Core ICMP Message Types

| Type  | Code |         Message Name          | Description                                                               | Techical Context & Use Case                                                  |
| :---: | :--: | :---------------------------: | ------------------------------------------------------------------------- | ---------------------------------------------------------------------------- |
|   0   |  0   |          Echo Reply           | Response to Echo Request ("I am alive")                                   | Baseline connectivity checks (`ping`)                                        |
|   8   |  0   |         Echo Request          | Polling message ("Are you alive?")                                        | Probing node availability (`ping`)                                           |
|   3   | 0-15 |    Destination Unreachable    | Packet cannot be delivered (bad route, unreachable part or MTU exceeeded) | Returned when a destanation node/port is closed or dropped by packet filters |
|   3   |  4   | Fragmentation Needed (DF Set) | Path  MTU blocked by a smaller MTU hop with Don't Fragment bit enabled    | Essential for **Path MTU Discovery (PMTUD)**                                 |
|  11   |  0   |         Time Exceeded         | Packet TTL dropped to zero in transit                                     | Prevents routing loops; foundational mechanic of `traceroute`                |
|  12   |  0   |       Parameter Problem       | Corrupted or unrecognized IP header field                                 | Processing error at an intermediate router or endpoint                       |
|   4   |  0   |         Source Quench         | Choke packet requesting the sender slow down                              | Deprecated / Obsolete.<br>Used in legacy networks for congestion control     |
|   5   | 0-3  |           Redirect            | Advises sender of a shorter/alternative gateway route                     | Informs local hosts about better next-hop gateways on the local subnet       |
| 13/14 |  0   |   Timestamp Request / Reply   | Request/Response containing millisecond timestamps                        | Latency measurements and clock synchronization testing                       |
- - -



- - -
More about ICMP: www.iana.org/assignments/icmp-parameters