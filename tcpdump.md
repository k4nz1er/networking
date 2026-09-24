- - -
tool: tcpdump

category: Packet Analyzer / CLI Sniffer

layer: 2-7 (OSI)

binary: /usr/bin/tcpdump

source: "https://www.tcpdump.org/"
- - -

# tcpdump

is an industry-standard command-line packet analyzer built on top of `libpcap`. It captures Layer 2-7 traffic passing through network interfaces, matching packets against in-kernel **BPF (Berkeley Packet Filters)** to inspect, filter and store network activity into standard `.pcap` files.

- - -

## Essential Execution Flags

|         Flag          | Function                                         | Operational Context                                                             |
| :-------------------: | ------------------------------------------------ | ------------------------------------------------------------------------------- |
|     `-i <iface>`      | Listen on a designated interface.                | Use `-i any` to capture on all interfaces simultaneously.                       |
|         `-n`          | Do not resolve hostname (IPs remain numeric).    | Critical: avoids generating recursive DNS traffic during capture.               |
|         `-nn`         | Do not resolve hostname **and** port names.      | Displays `80` instead of `http`, `443` instead of `https`.                      |
|         `-e`          | Print Layer 2 data link headers.                 | Prints source/destination MAC addresses and EtherType (`0x0800`, `0x0806`).     |
| `-v` / `-vv` / `-vvv` | Increase output verbosity.                       | Dissects TTL, IP ID, options, flags, and checksum verification status.          |
|     `-X` / `-XX`      | Print packet data in Hex and ASCII format.       | Useful for reading unencrypted application payloads (`-XX` includes L2 header). |
|      `-s <len>`       | Set snaplength (bytes of packet to capture).     | `-s 0` captures full un-truncated frames (default in modern versions).          |
|   `-w <file.pcap>`    | Write raw captured packets directly to disk.     | Skips console parsing; stores data for offline inspection in Wireshark.         |
|   `-r <file.pcap>`    | Read and parse previously recorded PCAP file.    | Replays offline traffic through BPF expressions.                                |
|     `-c <count>`      | Terminate execution after capturing $N$ packets. | Prevents runaway logging.                                                       |

- - -

## Berkeley Packet Filter (BPF) Syntax

BPF expressions filter traffic directly within kernel space, drastically reducing packet loss during high-volume throughput.

### Primitive Qualifiers:
* **Type:** `host`, `net`, `port`, `portrange`
* **Direction:** `src`, `dst`, `src or dst`, `src and dst`
* **Protocol:** `ether`, `ip`, `ip6`, `arp`, `icmp` and e.g.

### Logical Operators:
* **AND:** `and` or `&&`
* **OR:** `or` or `||`
* **NOT:** `not` or `!`

- - -

## High-Value Practice

### Layer 2
```bash
# Capture ARP requests and replies with MAC addresses visible
sudo tcpdump -nn -e -i any arp

# Capture traffic to/from a specific hardware MAC address
sudo tcpdump -nn -e -i any ether host ff:ff:ff:ff:ff:ff
```

### Layer 3
```bash
# Filter ICMP destination unreachable and time-exceeded messages
sudo tcpdump -nn -v -i any "icmp[icmptype] == 3 or icmp[icmptype] == 11"

# Exclude SSH traffic while monitoring a remote server IP
sudo tcpdump -nn -i any "host 192.168.1.50 and not port 22" 
```

### Layer 4
```bash
# Capture TCP SYN packets (Connection Initiation / Port Scans)
sudo tcpdump -nn -i any "tcp[tcpflags] & tcp-syn != 0 and tcp[tcpflags] & tcp-ack == 0"
```

### Forensic Export
```bash
# Capture all traffic into 100MB files, keeping the last 5 files in a rotating buffer
sudo tcpdump -nn -i eth0 -s 0 -C 100 -W 5 -w /var/log/traffic.pcap
```

- - -

