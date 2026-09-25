- - -
protocol: UDP

layer: 4 (Transport)

RFC 768

encapsulation: IPv4 (Protocol 17) / IPv6 (Next Header 17)

source: "Andrew S. Tanenbaum, Computer Networks (4th Edition), pp. 598-606"
- - -

# UDP (User Datagram Protocol)

is a minimal, connectionless Layer 4 transport protocol defined in **RFC 768**. It provides best-effort delivery of encapsulated IP datagrams without maintaining connection state, guaranteeing sequence delivery, handling flow control or performing automated retransmissions.

> **Fundamental Role:** UDP acts primarily as an application-level multiplexer/demultiplexer for IP, introducing port abstractions so multiple software processes on the same host can transmit and receive indent datagram streans.

- - -

# UDP Datagram Header Layout

The UDP header is fixed-size and takes exactly **8 bytes** (64 bits), followed immediately by the application payload.

```text
0                   1                   2                   3  
0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|         Source Port         |        Destination Port       |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|            Length           |            Checksum           |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

field breakdown:

|      Field Name      | Size    | Purpose & Mechanics                                                                                                                                                                                          |
| :------------------: | ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
|   **Source Port**    | 16 bits | Port on the transmitting machine. Used by the destination process to send replies (copied directly into the reply's Destination Port field). Set to `0` if no response is expected.                          |
| **Destination Port** | 16 bits | Demultiplexing endpoint identifier on the receiving host. Bound to a local operating system socket (via `bind()` system call).                                                                               |
|      **Length**      | 16 bits | Total datagram size in bytes (8-byte header + application payload). Minimum value is `8` (datagram with 0 bytes of payload). Maximum theoretical length is 65,535 bytes (bonded by IPv4 Total Length limit). |
|     **Checksum**     | 16 bits | Optional in IPv4: mandatory in IPv6. 1's complement sum covering a 12-byte Pseudo-Header (derived from the IPv4 header), the UDP header and the payload. If unused, the sender injects all zeros (`0x0000`). |
