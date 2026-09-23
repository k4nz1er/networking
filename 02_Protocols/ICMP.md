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

## Cybersecurity implications

* **Network Reconnaissance & Host Discovery:**
	* Threat actors use `Echo Request` (Type 8), `Timestamp` (Type 13) and `Address Mask Request` (Type 17) to enumerate live hosts across subnets.
	* Dropping incoming Type 8 messages at network boundaries conceals hosts from trivial discovery scans.
* **Traceroute Mechanics & Internsl Topology Leaks:**
	* Classic UDP/ICMP `traceroute` deliberately generates TTL values starting at `1`, sequentially triggering ICMP `Time Exceeded` (Type 11) replies from intermediate routers to map internal transit hops.
* **ICMP Tunneling & Data Exfiltration:**
	* Because ICMP `Echo Request/Reply` permits arbitrary payload padding after the 4-byte header, tools such as `iodine`, `ptunnel` or `Hans` can encapsulate arbitrary TCP/IP or C2 traffic inside Echo payloads to bypass perimeter firewalls that do not inspect packet contents.
* **ICMP Redirect Attacks (MITM):**
	* Attackers on the same local segment forge ICMP `Redirect` (Type 5) frames to convince a victim host to route its traffic through an attacker-controlled machine rather than the default gateway.
* **Black Hole Connections (Broken PMTUD):**
	* Firewalls that blindly drop all ICMP Type 3 Code 4 packets prevent PMTUD from operatin, causing TCP handshakes to succeed while larger HTTP/TLS data packets hang and drop indefinitely.

- - -
More about ICMP: www.iana.org/assignments/icmp-parameters