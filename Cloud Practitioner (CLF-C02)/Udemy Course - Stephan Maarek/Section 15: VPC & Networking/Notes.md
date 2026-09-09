# Section 15: VPC & Networking

## IP addresses in AWS
### IPv4
- Private IPv4 addresses are fixed for EC2 instances even if you start or stop them
- A new Public IPv4 address is assigned to an EC2 instance everytime you start/stop them
- Elastic IP - allows you to attach a fixed public IPv4 address to an EC2 instance
- All public IPv4 addresses on AWS will be charged $0.005 per hour (including EIP)

### IPv6
- Every IPv6 address in AWS is public
- Free

## VPC and Subnets
- VPC (virtual private cloud)
    - private network to deploy your resources
    - linked to a specific region
- Subnet
    - allows you to partition your netowrk inside your VPC
    - normal networking subnets - no special aws stuff to remember
- Route Table
    - define rules that allow subnets to communicate with each other or the public internet

## Internet Gateway and NAT Gateways
- Internet Gateways
    - help our VPC instances connect wit the internet
- Public subnets have a route to the internet gateway
- NAT Gateway (AWS Managed)
- Nat Instances (self-managed)
- NAT Gateways and NAT Instances allow your private subnet to access the internet while remaining public
- A NAT gets created in your public public subnet, and then your private subnets can go trhough it to communicate with the public internet while remaining private

## Network ACL (NACL)
- A firewall which controlls traffic from and to a subnet
- Can have ALLOW and DENY rules
- are attached at the Subnet level
- Rules only include IP addresses
- Is stateless: return traffic must be explicitly allowed by rules
- rules are evaluated in number order - first match decides behavior

## Security Groups
- A firewall that controls traffic to and from an EC2 Instance
- Can have only ALLOW rules
- Rules include IP addresses and other security groups
- are attached at the EC2 instance level
- Is stateful: Return traffic is allowed regardless of rules
- All rules are evaluated before traffic is allowed/denied

## VPC Flow Logs
- Capture information about IP traffic going into your interfaces
    - VPC flow log
    - Subnet flow log
    - Elastic Network Interface flow log
- Helps to monitor and troubleshoot connectivity issues
    - subnet to subnet
    - subnet to internet
    - internet to subnet
- Captures network information from AWS managed interfaces too
    - Elastic Load Balancers (ELB)
    - ElastiCache
    - RDS
    - Aurora
    - etc
- VPC flow logs data can go to S3, CloudWatch Logs, and Amazon Data Firehouse

## VPC Peering
- Connect two VPC, privately using AWS' network
- Make them behave as if they were in the same network
- Must not have overlapping CIDR (IP address range)
- VPC Peering connection is not transitive
    - If VPC 1 has VPC peering to VPC 2
    - And VPC 2 has VPC peering to VPC 1 and VPC 3
    - VPC 1 CANNOT communicate with VPC 3 over VPC 2's peer connection
    - Each VPC pair needs its own VPC peering

## VPC Endpoints

- Endpoints allow you to connect to AWS services using a private network instead of the public www network
- provides enhances security and lower latency to access AWS services

### VPC Endpoint Gateway
- For connecting to S3 and DynamoDB only

### VPC Endpoint Interface
- works for most services (including S3 and DynamoDB)

## AWS PrivateLink
- VPC Endpoint Service
- Most secure and scalable way to expose a service to 1000s of VPCs
- Does not require VPC peering, internet gateway, NAT, route tables...
- Allows consumers to easier and scalably connect to a 3rd party service on AWS privately
- Example:
    - 3rd party vendor creates a Network Load Balancer in their VPC (Service VPC)
    - Consumer creates and Elastic Network Interface in their VPC (consumer VPC)
    - an private link is established between the service VPC and consumer VPC 
    - All communications remain secure within the private VPC

## Site to Site VPN vs Direct Connect (DX) - For Hybrid Cloud
### Site to Site VPN - For Hybrid Cloud 
- Connect and on-premise VPN to AWS
- Connection is automatically encrypted
- Goes over public internet
- Can be setup quickly

#### Setup
- On-premise: must setup a Customer Gateway (CGW)
- AWS: must setup a virtual private gateway (VGW)
- All devices in the VPC communicate over these gateways

### Direct Connect (DX) - For Hybrid Cloud
- Establish a physical connection between on-premise and AWS
- The connection is private, secure, and fast
- Goes over private network
- Takes at least a month to establish connection

### Deciding Factors
- Does it need to be established fast?
- Does it need to communicate over a private network?

## AWS Client VPN
- Pretty straight forward VPN setup, I dont need to study this
- Connect from your computer using OpenVPN to your private network in AWS and on-premises
- Allows you to connect to your EC2 instances over a private IP (just as if you were in the private VPC)

## Transit Gateway Overview
- Provides transitive peering between thousands of VPC and on-premise
- hub-and-spoke style connection
- Much more simple than setting up thousands of peer to peer connections
- Works with AWS VPCs, Direct Connect Gateways, and VPN connections

## Summary
- VPC: Virtual private cloud
- Subnets: tied to an AZ, network partiton of the VPC
- Internet Gateway: as the VPC level. provides Internet Access
- NAT Gateway / Instances: give internet access to private subnets
- NACL: Stateless, subnet rules for inbound and outbound
- Security groups: Stateful, operate at the EC2 Instance level or ENI
- VPC Peering: Connect two VPC with non overlapping IP ranges, nontransitive
- Transit Gateway Overview: Connects thousands of VPC and on-premise networks together, transitive
- Elastic IP: fixed public IPv4, ongoing cost if not in-use
- VPC Endpoints: provides private access to AWS services within VPC
- PrivateLink: privately connect to a service in a 3rd party VPC
- VPC Flow logs: network traffic logs
- Site to Site VPN: Vpn over public internet between on-premises datacenter and AWS
- Client VPN: OpenVPN connection from your computer into you2r5 36PC
- Direct Connect: direct private connection to AWS












