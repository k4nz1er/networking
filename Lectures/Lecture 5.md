# IPv4 Addressing

**Every device** use a unique IP address
**Subnet mask** used by the local device to determine what subnet it's on
**Default gateway** - the router that allows you to communicate outside of your local subnet
**Loopback address** an address to yourself
**Reserved addresses** set aside for future use or testing (all "Class E" address)
**Virtual IP address (VIP)** virtual machine, internal router address

**4 octets**, since one byte is 8 bits, the maximum decimal value for each byte is 255

## DHCP (Dynamic Host Configuration Protocol):
IPv4 address configuration used to be a manual process
(IP address, subnet mask, gateway, DNS servers, NTP servers, etc.)

**APIPA (Automatic Private IP Addressing)**
a link-local address (can only communicate to other loacal devices)

## RFC 1918 private IPv4 address:

10.0.0.0 - 10.255.255.255        16.777.216     /8      24 bits

172.16.0.0 - 172.31.255.255      1.048.576      /12     20 bits

192.168.0.0 - 192.168.255.255    65.536         /16     16 bits


# Clasful Subnetting

Class **A**       255          0           0           0  
                11111111    00000000    00000000    00000000
                Network(8)      Hosts(24)

Class **B**       255         255          0           0
                11111111    11111111    00000000    00000000
                Network(16)     Hosts(16)

Class **C**       255         255         255          0
                11111111    11111111    11111111    00000000
                Network(24)    Hosts(8)



