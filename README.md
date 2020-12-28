# C002Notes

## VPC
- VPC Cannot span multiple Region
- VPC can span multiple Availability Zone (AZ) within a specific region
- There is upto 5 VPC that can be created within a account
- Logically isolated from outside and within a account
- You cannot change the CIDR size (up or down) once VPC has been created

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
- Each Subnet must be associated with only ONE route table at any given point in time
- Different subnet within a VPC CANNOT overlap

## Route Tables
- In a VPC there is a limit of upto 200 route tables
- in each route table you can add upto 50 route entries
- A single route table may be attached to multiple Subnets
