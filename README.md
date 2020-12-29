# C002Notes
Below are my notes while doing prep for AWS CAA-002 exams. All these notes are from watching various courses (such as ACG, EduFin, Dojo etc). These notes are free to use, I just aggregated it one place for reference



## VPC 
- VPC Cannot span multiple Region
- VPC can span multiple Availability Zone (AZ) within a specific region
- There is upto 5 VPC that can be created within a account
- Logically isolated from outside and within a account
- You cannot change the CIDR size (up or down) of an existing CIDR
- You CAN increase the VPC size
- Largest VPC is 0.0.0.0/16
- Smallest VPC is 0.0.0.0/28

#### Components of VPC
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
- AWS reserves 5 address in each subnet (first 4 and last 1) e.g. 10.0.0.0/24 - 10.0.0.0,10.0.0.1,10.0.0.2, 10.0.0.3 and 10.0.0.255 are reserved

## Route Tables
- In a VPC there is a limit of upto 200 route tables
- in each route table you can add upto 50 route entries
- A single route table may be attached to multiple Subnets


## Security Group (SG)
- Security Group are also referred as Virtual Firewall
- SG are stateful
- SG are directional and can only use allow rules only
- You can have upto 5 SG attached to a single EC2 instance
- You can allow but cannot define DENY (hence stateful - logical deduction)
- there is implicit DENY for any other roles
- User cannot delete default security group
- Security group are VPC resources (i.e. they do not span across VPC)
- Same security group can be tied to multiple instances and multiple AZs

Assumption here is that VPC is still in default setting (nothing has changed in SG setting)
- In Default Security Group
  - All outbound traffic is allowed by default i.e., there ia an existing rule in SG Outbound tab which says all traffic (protocols) from all ports is allowed to hit 0.0.0.0/0
  - All incoming traffic (all ports and all protocols) from any device (EC2 or ENI etc) which is tied with same security group (default security group in this case) is allowed
  - User cannot delete this SG
- In Custom Security Group (not default)
  - All outbound traffic is allowed by default i.e., there ia an existing rule in SG Outbound tab which says all traffic (protocols) from all ports is allowed to hit 0.0.0.0/0
  - have no inbound rules - All inbound traffic is denied by default
  - User can delete this SG once all dependencies are removed and this is not marked as default
  
  example
  Let say we have two EC2 instances in two different subnets tied with two different security group
  - Instance A EC2 can send message to B successfully
  - However when Instance B send message they are never received on A
  The problem is never on outgoing side (i.e., there is likely NOT an issue with instance A) but there may be blocks within SG of A which may be preventing this communication
  or the NACL since they are on different Subnets
