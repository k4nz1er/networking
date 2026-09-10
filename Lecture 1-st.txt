What is the OSI model ?
OSI (Open Systems Interconnection Reference Model)

Layer 1 - Physical (cables, fiber and the signal itself)
Layer 2 - Data Link (frame, MAC address, Extended Unique Identifier (EUI-48, EUI-64), switch)
Layer 3 - Network Layer (IP address, router, packet)
Layer 4 - Transport Layer (TCP segment, UDP datagram)
Layer 5 - Session Layer (control protocols, tunneling protocols)
Layer 6 - Presentation Layer (application enccryption (SSL/TLS))
Layer 7 - Application Layer (eyes)

CDN (Content Delivery Network) - geographically distributed caching server.

QoS (Quality of Service) - traffic shaping, packet shaping, control by bandwidth usage or data rates.

IP (Internet Protocol):
 ___________ _________________ ___________________ _______________
|  Version  |  Header Length  |  Type of Service  | Total Length  |
 ----------- ----------------- ------------------- ---------------
|        Identificate        |    Flags    |    Fragment Offset   |
 ---------------------------- ------------- ----------------------
|        Time to Live        |   Protocol  |    Header Checksum   |
 ---------------------------- ------------- ----------------------
|                          Source IP Address                      |
 -----------------------------------------------------------------
|                        Destination IP Address                   |
 -----------------------------------------------------------------
|                         Options and Padding                     |
 -----------------------------------------------------------------

DNS (Domain Name System)

SaaS (Software as a service) - on-demand software, central management of data and application

IaaS (Infrastructure as a service)

PaaS (Platform as a service)


TCP (Transmission Control Protocol) 

UDP (User Datagram Protocol)

FTP (File Transfer Protocol) - port 20 (active mode data) and 21 (control)

SSH (Secure Shell) - port 22

SFTP (Secure FTP)

Telnet (Telecommunication Network) - port 23

SMTP (Simple Mail Transfer Protocol) - port 25 (SMTP using plaintext) and 587 (SMTP using TLS encryption)

DNS (Domain Name System) - port 53 (udp)

DHCP (Dynamic Host Configuration Protocol) - automated configuration of IP address, subnet mask and other options, port 67 and 68

TFTP (Trivial File Transfer Protocol) - port 69

HTTP (Hypertext Transfer Protocol) and HTTPS (HTTP Secure) - port 80 and port 443

NTP (Network Time Protocol) - port 123 (udp)

SNMP (Simple Network Management Protocol) - gather statistic from network devices, port 161 (udp)

LDAP (Lightweight Directory Access Protocol) - store and retieve information in a network directory port 389

LDAPS (LDAP Secure) - SSL on port 636

SMB (Server Message Block) - direct over port 445 (NetBIOS-less)

Syslog (Standard for message logging) - port 514 (udp)

Microsoft SQL (Structured Query Language) Server - port 1433

RDP (Remote Desktop Protocol) - share a desktop from a remote location over port 3389

SIP (Session Initiation Protocol) - Voice over IP (VoIP) signaling port 5060 and 5061
