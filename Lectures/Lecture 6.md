# Calculating IPv4, subnets and hosts

**VLSM (Variable Length Subnet Masks)**
- class-based networks are inefficient
- allow network administrators to define their own masks
- use different subnet masks in the same classful network

Calculating subnets and hosts:
Number of subnets = 2 ** subnet nits
Hosts per subnets = 2 ** host bits - 2

10.1.1.0/24

Network = 8 bits, Subnet = 16 bits, Host = 8 bits

11111111.11111111.11111111.00000000

Total subnets = 16 bits = 2 ** 16 = 65.536

Hosts per Subnet = 8 bits = 2 ** 8 - 2 = 256-2 = 254

192.168.11.0/26

Network = 24 bits, Subnet = 2 bits, Host = 6 bits

11111111.11111111.11111111.11000000

Total subnets = 2 bits = 2 ** 2 = 4

Hosts per Subnet = 6 bits = 2 ** 6 - 2 = 64 - 2 = 62

172.16.55.0/21

Network = 16 bits, Subnet = 5 bits, Host = 11 bits

11111111.11111111.11111000.00000000

Total subnets = 5 bits = 2 ** 5 = 32

Hosts per Subnet = 11 bits = 2 ** 11 - 2 = 2.048 - 2 = 2.046



# Magic Number Subnetting

## Four important addresses

**Network address/subnet ID** - the first address in the subnet
**Broadcast address** - the last address in the subnet
**Firest available host address** - one more than the network address
**Last available host address** - one less than the broadcast address

