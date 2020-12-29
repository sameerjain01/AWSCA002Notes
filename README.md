# C002Notes
Below are my notes while doing prep for AWS CAA-002 exams. All these notes are from watching various courses (such as ACG, DolfinEd, Dojo, Udemy etc). These notes are free to use, I just aggregated it one place for reference

For more accurate details I refer to AWS documentation 

For AWS Quotaa best reference is [AWS official documentation](https://docs.aws.amazon.com/vpc/latest/userguide/amazon-vpc-limits.html)


## VPC 
- VPC Cannot span multiple Region
- VPC can span multiple Availability Zone (AZ) within a specific region
- There is upto 5 VPC that can be created within a account
- Logically isolated from outside and within a account
- You cannot change the CIDR size (up or down) of an existing CIDR
- You CAN increase the VPC size
- Largest VPC is 0.0.0.0/16
- Smallest VPC is 0.0.0.0/28
- Quota
  - IPv4 CIDR block per VPC is 5
  - IPv6 CIDR block per VPC is 1
  

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
- Quota for Subnets per VPC is 200


## Route Tables
- In a VPC there is a limit of upto 200 route tables
- in each route table you can add upto 50 route entries
- A single route table may be attached to multiple Subnets
- Quotas
  - Routes tables per VPC - 200
  - Routes per route table (non-propagated routes) - 500 )extendible upto 1000 however with bad performance)
  - BGP advertozed routes per route table (propagated routes) - 100 (cannot be increased. 100 is hard limit)

## Security Group (SG)
More Accurate Details can always be found at [AWS Security Group Official Documentation](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html)

- Security Group are also referred as Virtual Firewall
- SG are stateful
- SG are directional and can only use allow rules only
- You can have upto 5 SG attached to a single EC2 instance
- A security group set of rules end with an implicit deny any
- You can allow but cannot define DENY (hence stateful - logical deduction)
- there is implicit DENY for any other roles
- User cannot delete default security group
- Security group are VPC resources (i.e. they do not span across VPC)
- Same security group can be tied to multiple instances and multiple AZs
- Subnet ID cannot be used as Source in Inbound Rules
- You can specify separate rules for inbound and outbound traffic.
- Security group rules enable you to filter traffic based on protocols and port numbers.
- When you create a new security group, it has no inbound rules. Therefore, no inbound traffic originating from another host to your instance is allowed until you add inbound rules to the security group.

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
  
  #### [Security Group Quotas](https://docs.aws.amazon.com/vpc/latest/userguide/amazon-vpc-limits.html)
  - VPC Security Group Per Regions - 2500
  - Inbound or Outbound rules per Security Group - 60
  - Security Group Per Network Interface
  
## Network Access Control List or NACL
  - A default NACL (created when VPC is created) allows all traffic inbound and outbound
  - A Custom NACL (cretaed by user) DENIES all traffic inbound and outbound by explicit deny
  

## NAT instance
  - Can work with both Elastic IP Address and Public IP Address
  - Must be always in Public Subnet
  - Always turn off the Source destination check on the instance for the traffic (Outgoing from private subnet to internet & Incoming traffic from internet to private subnet)
  - AWS does not patch ot update the instance, you will need to perform updates. PLease allow port 80 SSH/RDP for admin activity
  - EC2 Instances in Public Subnet do NOT need to access the NAT Instance since they have direct access to internet by IGW and public IP address
  - Not Horizantally scaled
  - NAT Instance SG must allow
    - inbound from Private Subnet or private subnet's Security Group as Source for port 80 and 443
    - outbound to 0.0.0.0/0 (internet) on Port 80 & 443
    - Traffic inboud from user own network on port 22 to administer
      - if you open this to 0.0.0.0/0 anyone in the world can access your NAT instance, thus a security risk 
   
## NAT Gateway
  - AWS managed service
  - Horizontally scalable and redundant
  - ONLY works with Elastic IP address
  - Cannot be assigned Security Group
  - Must always be created in a Public Subnet
  
  
  #### Monitoring
  The NAT Gateways has several monitoring on dashboard
  - Count of Packets out to destination
  - Count of Packets out to Source
  - Count of Packet drops
  - Count of Bytes out to destination
  - Count of Bytes out to Source
  - Count of Error port allocation
  - Count of Active Connections
  - Count of Connections Attempt
  - Count of Connections Established
  - Cound of Idle Timeouts
  
## Misc
  - Ephemeral port range is between 1024 - 65535, (any application (usually browser) that is requesting response can use this port)
