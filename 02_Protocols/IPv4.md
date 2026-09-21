
#  Protocol IP

---
layer: 3 (Network - OSI)
RFC 791
- - -
IPv4 is a connectionless, best-effort Layer3 (OSI Model) protocol responsible for packet addressing, routing and fragmentation across packet-switched networks.
- - -
## IPv4 Datagram Header Layout

The standard IPv4 header has a minimum size of **20 bytes** (without options) and can scale up to **60 bytes** when options are present.
```text
0                   1                   2                   3
0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|Version|  IHL  |Type of Service|         Total Length        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|         Identification        |Flags|    Fragment Offset    |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|  Time to Live  |   Protocol   |        Header Checksum      |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                        Source IP Address                    |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                     Destanation IP Address                  |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                    Options                    |   Padding   |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```
- - -
## Header Fields Breakdown

|              Field              | Size (Bits) | Description                                                                                                                                                                                          |
| :-----------------------------: | :---------: | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|             Version             |      4      | Identifies the IP protocol version.<br>Always 4 (0100 in binary).                                                                                                                                    |
| IHL<br>(Internet Header Length) |      4      | Specifies the length of the header in 32-bit (4-byte) words. Minimum value is `5` (5x4=20 bytes); maximum is `15` (15x4=60 bytes).                                                                   |
|         Type of Service         |      8      | Traffic prioritization and QoS parameters. Divided into DSCP (Differentiated Services Code Point) and ECN (Explicit Congestion Notification).                                                        |
|          Total Length           |     16      | Total size of the datagram (header + payload) in bytes. Maximum size is 2^16 - 1 = 65.535 bytes (ports).                                                                                             |
|         Identification          |     16      | Unique ID assigned to a datagram to enable reassembly of fragmented packets at the **destination** host.                                                                                             |
|              Flags              |      3      | Controls fragmentation:<br><br>- Bit 0: Reserved (must be zero);<br><br>- Bit 1: DF (Don't Fragment, returns ICMP if payload exceeds MTU);<br><br>- Bit 2: MF (More Fragments, except the final one) |
|                                 |             |                                                                                                                                                                                                      |
