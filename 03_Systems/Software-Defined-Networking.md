- - -
concetp: SDN & SD-WAN

layer: Infrastructure / Network Architecture

source: "https://yotu.be/A6tPNGEfOzc?is=aWuzhrGSxxDWBal4"
- - -
# SDN (Software-Defined-Networking)

**Software-Defined-Networking (SDN)** decouples the network control logic from the underlying physical hardware, enabling dynamic, programmatically efficient network configuration and central management.
- - -
## The Three Planes of Networking

Traditional network appliances (routers, firewalls, switches) integrate all functions into a single hardware chassis. SDN decomposes this functionality into three discrate architectural layers:
```text
+--------------------------------------------------------------------+
|   Application / Management Plane (APIs, GUIs, SSH, Orchestrators)  |
+--------------------------------------------------------------------+
                              |  (Northbound APIs)
+--------------------------------------------------------------------+
|      Control Plane (Routing Tables, FIB, NAT Logic, Policies)      |
+--------------------------------------------------------------------+
                              |  (Southbound APIs - e.g., OpenFlow)
+--------------------------------------------------------------------+
|      Data / Infrastructure Plane (Hardware Interfaces, ASICs)      |
```

 
|                Plane                 | Core Role & Repsonsibilities                                                                                      | Concrete Examples                                                   |
| :----------------------------------: | ----------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------- |
|  Infrastructure layer / Data plane   | Handless the physical transit of packets. Performs forwarding, trunking, packet encryption, segmentation and NAT. | Physical interfaces switch ports, network ASICs.                    |
|    Control layer / Control plane     | Determines **where** and **how** traffic should be routed and filtered. Manages states and policies.              | Routing tables (RIB), MAC forwarding tables, firewall state tables. |
| Application layer / Management plane | Interface for network engineers and automated software to configure and monitor devices.                          | Web UIs, SSH CLI, REST APIs, central controllers.                   |
- - -
## SD-WAN (Software-Defined WAN)

**Software-Defined WAN (SD-WAN)** - applies SDN principles to Wide Area Network (WAN), routing traffic dynamically across hybrid links to address distributed and multi-cloud architectures.

**Key Capabilities:**
- **Application-Aware Routing:** inspects Layer 7 payloads to identify applications (e.g. VoIP vs. bulk backups) and steers them over the optimal transport path.
- **Transport Agnostic:** operates interchangeably across fiber, broadband, 5G, MPLS or LTE.
- **Zero-Touch Provisioning (ZTP):** edge routers download cryptographic keys and configuration policies automatically upon connecting to the Internet.
- **Central Policy Management:** policies are defined in a unified controller and pushed globally to all edge appliances.
- - -
## Security Implications
(Threat & Audit Perspective)

**Central Point of Failure / Compromise:**

The centralized SDN controller is a primary target. A compromised controller gives an adversary arbitrary control over routing tables, allowing silent traffic sniffing, MITM or network-wide isolation.

**API Security (Northbound/Southbound):**

Unauthenticated or poorly secured REST APIs on controllers can lead to unauthorized network reconfiguration.

**Telemetry & Microsegmentation:**

SDN provides granular microsegmentation (isolating workloads within virtual subnets) and centralized traffic telemetry for SIEM and NDR sensors.
- - -


