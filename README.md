# C002Notes

## VPC
- VPC Cannot span multiple Region
- VPC can span multiple Availability Zone (AZ) within a specific region
- There is upto 5 VPC that can be created within a account
- Logically isolated from outside and within a account

### Components of VPC
- CIDR and IP Address (v4 & v6)
- Subnet
- Route tables
- Load Balancers
- Network access control layer (NACL)
- Availability Zone
- NAT Gateway & NAT Instance - works at Subnet level - Stateless
- Internet Gateways
- Security Firewall (attached to instances) - Stateful - works at Elastic Network Interfact(ENI) (virtual)
- Transit Gateways
- Peering Connections
- Implied logical rounter


## Subnet
- Subnet is tied to a specific AZ within a VPC within a region
- Subnet cannot span multiple AZ
- Subnet cannot span multiple VPC
- Subnet cannot span multiple Region
- Subnet has 1:N relationship with AZ (In a given AZ we can have multiple subnets)
