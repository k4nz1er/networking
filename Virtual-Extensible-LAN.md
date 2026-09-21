
Data center interconnect (DCI)

- Connect multiple data centers together
	- seamlessly span across these geographic distances
- Connect and segment different customer networks
	- across multiple data centers
	- all customers share the same core network
- Distribute applications everywhere
	- increase uptime and availability
	- workload can be moved to the best location
- IP addressing is different across data centers
	- challenging to manage dynamically created virtual systems
- Data centers can be connected in different ways
	- MAPLS, high speed optical, Metro Ethernet etc.
- Application shouldn't have to worry about IP addressing, routing or connectivity
	- Applications should work regardless of physical location
- Extend networks across physical locations
	- encapsulate, send the data, decapsulate
	- tunnel the data
- Designed for large service providers
	- hundreds or thousands of tenants
- VLANs
	- Maximum of about 4000 possible virtual networks
	- Fixed layer 2 domain
	- Not designed for large scale and dynamic movement of VMs
- VXLAN support
	- over 16 million possible virtual networks
	- tunnel frames across a layer 3 netwrok
	- built to accommodate large virtual environments