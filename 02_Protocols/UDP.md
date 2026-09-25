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

## UDP Datagram Header Layout

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
- - -

## What UDP does NOT do (By Design)

* **No connection estabilshment:** no handshakes (e.g., unlike **TCP** SYN/ACK); datagrams are transmitted immediately upon application request.
* **No flow or congestion control:** transmitters push packets onto the wire regardless of intermediate router buffer saturation or receiver queue drops.
* **No in-order sequencing:** packets traversing varied network paths may arrive out-of-order; UDP presents no sequence numbers to reorder them.
* **No automatic retransmission:** corrupted or dropped datagrams are silently discarded. Reliability, timeout handling and flow-control algorithms must be explicitly managed by user-space applications if needed.

- - -

## Practical implementations & higher-layer paradigms

Because UDP introduces almost no overhead, it is used in environments prioritizing low latency over absolute reliability:

**A. Request-Reply & Client-Server rotocols**
* Protocols such as [[DNS]] and [[DNCP]] execute short transactions. A client sends a single request and awaits a single answer. If a response times out, the application simply retransmits the query, saving round-trip handshakes.

**B. RPC (Remote Procedure Call)**
* Formulated by Birrell and Nelson (1984) to make network requests look like standard local function calls.
* Relies on **Client Stubs** (marshaling arguments into UDP packets) and **Server Stubs** (demarshaling and invoking server processes).
* Requires careful handling for non-idempotent operations (operations where re-executing a dropped request causes state corruption, such as banking balance updates).

**C. Real-Time Transport Protocol (RTP) & RTCP (RFC 1889)**

Designed for multimedia streaming, VoIP and video conferencing running in user space over UDP:
* **RTP (Data Transport):** multiplexes streams into UDP packets, appending timestamps (to counter network jitter) and sequence numbers (to detect dropped packets without pausing for retransmissions).
* **RTCP (Control Protocol):** periodically exchanges diagnostic telemetry (packet loss rates, jitter buffers, transmission delay) to help encoders dynamically adjust video/audio bitrates to fit bandwidth limitations.
- - -

## Cybersecurity Implications

|      Offensive Technique       | Mechanism                                                                                                                                                                                                                                   | Deffensive                                                                                                   |
| :----------------------------: | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------ |
|  **DDoS (UDP Amplification)**  | UPD addresses are stateless and easily spoofed. Attackers forge victim IP addresses when requesting small payloads from public reflectrors ([[DNS]], [[NTP]], [[SNMP]]), causing massive amplified response streams directed at the target. | BCP 38 ingress source filtering; rate-limiting; disabling unused UDP services.                               |
|     **UDP Port Scanning**      | Scanner emits empty UDP probes to target ports. Open ports usually return nothinng (or an application reply), while closed ports return `ICMP Port Unreachable` (Type 3, Code 3).                                                           | Ingress firewalls dropping incoming unknown UDP traffic; host-based rate-limiting on ICMP unreachables.      |
| **Data Exfiltration over UDP** | Bypasses stateful TCP connection monitors by fragmenting or tunneling data through allowed UDP ports (e.g. DNS port 53, NTP port 123, VXLAN port 4789).                                                                                     | Next-Generation Firewall (NGFW) deep packet inspection (DPI) verifying true application protocol compliance. |
- - -
