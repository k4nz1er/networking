
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
|         Fragment Offset         |     13      | Measures the position of the fragment data relative to the start of the original datagram in units of 8-byte (64-bit) bloks. Max offset is 8192 blocks.                                              |
|       Time to Live (TTL)        |      8      | Hop counter designed to prevent routing loops. Decremented by >= 1 at each router hop;<br>when it reaches 0, the datagram is dropped and an `ICMP Time Exceeded` is generated.                       |
|            Protocol             |      8      | Identifies the encapsulated Layer 4 protocol (e.g. 1 for [[ICMP]], 6 for [[TCP]], 17 for [[UDP]]). Defined by IANA - RFC 1700.                                                                       |
|         Header Checksum         |     16      | 1's complement checksum computed over the **header only** (recomputed at every hop due to TTL decrements).                                                                                           |
|        Source IP Address        |     32      | IPv4 address of the originating sender.                                                                                                                                                              |
|     Destination IP Address      |     32      | IPv4 address of the final recipient.                                                                                                                                                                 |
|        Options & Paddig         |  Variable   | Optional features. Padded with zeroes to align on a 32-bit boundary.                                                                                                                                 |
- - -
## Optional features IPv4 datagram

|         Type          | Description                                                            |
| :-------------------: | ---------------------------------------------------------------------- |
|       Security        | Specifies the security classification level of the datagram            |
| Loose Source Routing  | Defines a list of mandatory routers that the datagram must visit       |
| Strict Source Routing | Defines the exact sequence of IP addresses the datagram must follow    |
|     Record Route      | Instructs routers to record their IP addresses into the header         |
|       Timestamp       | Instructs touters to record their IP addresses and current timestamps. |
- - -
## Security & Engineering Implications

- **OS Fingerprinting via TTL:**
	- Operating systems initialize the TTL field with distinct default values:
		- Linux/Android: 64;
		- Windows: 128;
		- Network equipment (Cisco): 255.
	* This allows passive OS identification in tools like `nmap` and `p0f`.

* **Fragmentation attacks:**
	* **Teardrop Attack**: Overlapping `Fragment Offset` values cause memory exhaustion or kernel panics during reassembly;
	* **IDS/IPS Evasion**: Crafting deliberately fragmented packets to bypass signature inspection on stateful firewalls.
* **Source Routing Risks:**
	* The `Strict Source Routing` and `Loose Source Routing` options allow the sender to predetermine the hop path, which can bypass network boundary, filters. For this reason, modern routers drop packets containing source routing options.
- - -
source: **Computer networks, 4 global edition, A. Tanenbaum (p. 498-501)**