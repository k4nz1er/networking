
# Protocol VXLAN

- - -
layer: Overlay (L2 over L4/L3; )

RFC 7348

standart port: UPD/4789 (IANA)
- - -

## VXLAN (Virtual Extensible LAN)

**Virtual Extensible LAN (VXLAN)** - is an encapsulation and network virtualization technology designed for large-scale multi-tenanat cloud environments and Data Center Interconnection (DCI). It creates an **overlay network** by tunneling Layer 2 Ethernet frames inside Layer 3/4 IPv4/UDP packets.
- - -

## VLAN vs. VXLAN

|      Feature      | VLAN (802.1Q)                              | VXLAN (RFC 7348)                                      |
| :---------------: | ------------------------------------------ | ----------------------------------------------------- |
| Identifier Length | 12 bits (VLAN ID)                          | 24 bits (VNI - VXLAN Network Identifier)              |
|   Maximum Scale   | $2^{12}=4,096$ segments                    | $2^{24} \approx 16.7$ million segments                |
|  Transport Scope  | Layer 2 only (non-routable across subnets) | Layer 3/4 routable (tunnels over IP/UDP)              |
|     Use Case      | Campus / enterprise broadcast isolation    | Multi-tenant cloud data centers, virtualized overlays |
- - -

## Core Architecture & Components

* **VNI (VXLAN Network Identifier):** 24-bit segment ID separating individual virtual broadcast domains.
* **VTEP (VXLAN Tunnel Endpoint):**
	* the entity that performs packet encapsulation and decapsulation
	* can be implemented in hardware (Top-of-Rack physical switch) or software (Hypervisor virtual switch like Open vSwitch)
* **Underlay vs. Overlay:**
	* Underlay: the physical routed IP network providing transport between VTEPs
	* Overlay: the virtual L2 network perceived by virtual machines (VMs communicate as if on the same local switch)
- - -

## Encapsulation Frame Format (Mac-in-UDP)

An original L2 frame is wrapped with headers before traversing the underlay:
```text
+-----------------+-------------+--------------+------------+--------------------+
|Outer L2 Ethernet|Outer IPv4/v6|Outer UDP 4789|VXLAN Header|Inner Original Frame|
|  Header (MACs)  |Header (VTEP)|    Header    |(24-bit VNI)|(L2 MAC + IP + Data)|
+-----------------+-------------+--------------+------------+--------------------+
|<---------------Underlay Transport Header--------->|<------Tenant Payload------>|
```

**Overhead & MTU Consideration:**

VXLAN encapsulation adds 50 bytes of overhead (14B Ethernet + 20B IP + 88B UDP + 8B VXLAN).

The physical underlay network requires Jumbo Frames (MTU >= 1550 or 1600 bytes) to prevent packet fragmentation.
- - -

## Security & Penetration Testing Implications

- **Cleartext Tunnel Traffic:**
	- by default, VXLAN does **not** enctyrpt tenant payloads. Anyone capturing underlay packets (e.g., via port mirroring or router compromise) can read all inner Ethernet/IP communications
	- mitigation: combine with [[IPsec]] or MACsec on underlay links
- **VNI Hopping/Injection:**
	- if a VTEP lacks strict ingress validation, an attacker capable of forging outer UDP packets directed to port 4789 can inject frames into arbitrary tenant VNIs
- **Network Visibility & Blind Spots:**
	- Legacy IDS/IPS sensors taht do not parse VXLAN encapsulation will only see UDP/4789 traffic, missing payload-based exploits happening inside the tenant's overlay.
- - -
