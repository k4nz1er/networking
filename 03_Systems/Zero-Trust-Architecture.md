- - -
concept: Zero Trust & SASE
- - -

## Zero Trust Fundamentals

Traditional security ("castle-and-moat") protected only the network edge-everything inside the perimeter was implicitly trusted. 
**Zero Trust** remove this assumption: **no user or device is trusted by default**.

* **Core rule:** *Never trust, always verify*
* **Principle of Least Privilege:** users receive only the minimum permissions necessary to complete their job tasks. If a role only requires reading a database, write access is denied.
* **Elimination of Blanket Admin Rights:** running everyday tasks as an administrator increases exposure. If a local machine is compromised, the malware inherits full administrative control across the environment.
- - -

## Adaptive Identity & Context-Aware Authentication

Instead of relying solely on username and password validation, the authentication engine continuously evaluates risk based on context:

* **Identity:** full-time employee vs. third-party contractor or vendor;
* **Location:** corporate campus vs. unknown external IP, unexpected geographic region or commercial VPN;
* **Time of access:** standard working hours vs. off-hours access attempts;
* **Device health:** managed corporate device with a valid machine certificate vs. unmanaged personal hardware.

If risk indicators spike (e.g., a login attempt from an overseas IP during the night), the system triggers step-up multi-factor authentication (MFA) or rejects the request entirely.
- - - 

## SASE (Secure Access Service Edge)

**SASE** unifies software-defined wide area networking (**SD-WAN**) and coud-native security services into a single architecture, superseding legacy VPN concentrators.

Rather than forcing all remote worker traffic through an internal headquarters bottleneck via VPN, inspection and policy enforcement occur in cloud points of presence (PoPs) adjacent to the user:

* An agent (**SASE Client**) runs on the endpoint.
* Traffic connects automatically to the nearest cloud edge broker.
* Security policies are enforced on-the-fly before passing traffic directly to target services (AWS, Azure, M365 or internal servers).

### Core  Components of the SASE Stack:
* **ZTNA (Zero Trust Network Access):** grants access to specific applications rather than exposing the underlying network segment.
* **FWaaS (Firewall as a Service):** centralized stateful packet and layer 7 inspection in the cloud.
* **SWG (Secure Web Gateway):** real-time URL filtering, malicious domain blocking, and threat inspection.
* **CASB (Cloud Access Security Broker):** data loss prevention and policy enforcement across SaaS platforms.
- - -

## Practical Security Takeaways

* **Credential Theft Mitigation:** compromised credentials obtained via phishing fail contextual challenges (e.g., unrecognized device, anomalous location).
* **Lateral Movement Prevention:** flat networks aallow simple enumeration and pivoting with tools like `nmap`. Under Zero Trust and ZTNA, endpoints cannot freely discover or connect to neighboring systems.
* **Physical Port Protection:** an unauthorized device plugged into a physical switch port is denied access to sensitive internal resources without  valid 802.1X machine authentication and posture validation.
- - -

## Related Notes

Network transport: [[Software-Defined-Networking]], [[Virtual-Extensible-LAN]]

Identity & Directory Services: [[LDAP]], [[Kerberos]]