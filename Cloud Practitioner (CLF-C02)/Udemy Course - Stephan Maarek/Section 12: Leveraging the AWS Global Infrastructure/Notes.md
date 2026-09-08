# Section 12: Leveraging the AWS Global Infrastructure

## Why Global Applications
- application is deployed in multiple geographies (regions / edge locations)
- decreased latency
- disaster recovery
    - if a region goes down you can fallback to another region
- attack protection: distributed global infrastructure is harder to attack

## Global AWS Infrastructure
- Regions: area for deploying applications and infrastructure
- Availability Zones:
    - physical data centers within a region
    - each region is made up of at least 3 AZs
- Edge Location:
    - also called "Points of Presence"
    - for content delivery as close as possible to users

## Global Application in AWS
- Global DNS: Route 53
    - routes users to the closest deployment with lowest latency
    - greate for disaster recovery strategies
- Global Content Delivery Network (CDN): CloudFront
    - Replicate part of your application to AWS edge locations to decrease latency
    - Cache common requests - improved user experience and decrease latency
- S3 Transfer Acceleration
    - Accelerate global uploads and downloads into Amazon S3
- AWS Global Accelerator
    - Improve global application availability and performance using the AWS global network

## Amazon Route 53
- Managed DNS
- Most common record types in AWS:
    - www.google.com > 12.34.56.78 == A record (IPv4)
    - www.google.com > 2001:...:7334 == AAAA record (IPv6)
    - search.google.com > www.google.com == CNAME (hostname to hostname)
    - example.com > AWS resource == Alias record

### Route 53 Routing policies
- Simple routing policy
    - No health checks
    - server gets a hostname > responses with a an IP address
- Weighted routing policy
    - allows distributed routing
    - acts as a sort of load balancer
    - instances are assigned weights, DNS server will distribute responses based on an instances weight and current traffic across all instances
- Latency routing policy
    - Users are routed to the IP address of the server with the lowerest latency
- Failover routing policy
    - disaster recovery
    - does health check on primary, if heath check succeeds route to primary, if it fails route to failover


## AWS CloudFront
- Content Delivery Network (CDN)
- improves read performance by caching content at edge locations
- improves user experience through lower latency
- hundred of points or presence globally
- DDoS protection (beacuse worldwide), integration with Shield, AWS Web Application Firewall

### CloudFront Origins
- backends the cloudfront can connect to
- Examples:
    - S3 bucket
        - for distributing files and caching them at the edge
        - for uploading files to S3 through CloudFront
        - Secured using Origin Access Controll (OAC)
    - VPC Origin
        - Applications hosted in VPC private subnets
        - Private application load balancer / Network Load Balancer / EC2 Instances
    - Custom Origin (over HTTP)
        - S3 website (must first enable the bucket as a static S3 website)
        - Any public HTTP backend you want

## S3 Transfer Acceleration
- Increase transfer speed by transferring file to an AWS edge location which will forward the data to the S3 bucket in the target region
- Speeds up cross region S3 downloads and uploads

## AWS Global Accelerator
- Improve global application availability and performance using the AWS global network
- Leverage the AWS internal network to optimize the route to your application (60% Improvement)
    - reduces hops
- Users connect to the closest Public AWS edge location and then traffic is routed through the private AWS network to the destination
- 2 Anycast IPs are created for your application

## Global Accelerator vs CloudFront
- CloudFront
    - CDN
    - Improves performance for your cacheable content
    - Content is served at the edge
- Global Accelerator
    - No caching, edge proxies packets to an application running in one or more aws regions
    - Improves performance for a wide range of application over TCP and UDP
    - Good for HTTP cases that require a static IP address
    - Good for HTTP cases that require deterministic, fast regional failover

## AWS Outposts
- offers the same AWS infrastructure, services, APIs, and tools to build your own applications on-premise just as in the cloud
- very useful for hybrid cloud setups
- AWS will setup and manage physical "Outpost Racks" within your on-premise infrastructure
- Shared responsibility:
    - YOU become responsible for physical security
- Benefits:
    - low latency access to on premise servers
    - local data processing
    - data residency
    - easier migration from on-premises to the cloud
    - fully managed service
- Some supported services:
    - EC2
    - EBS
    - S3
    - EKS
    - ECS
    - RDS
    - EMR

## AWS Wavelength
- Wavelength zones are infrastructure deployments embedded within the telecommunications providers' datacenters at the edge of the 5G networks
- Brings AWS services to the edge of 5G networks
- Ultra low latency over 5G networks
- No additional charges or service agreements
- EXAM NOTE: 5G service = wavelength.

## AWS Local Zones
- Place AWS compute, storage, database, and other selected AWS services closer to end users to run latency-sensitive applications
- Extend your VPC  to more locations - "Extensions of an AWS region"
- Must be enabled in the region's zone settings
- Example:
    - AWS Region: N. Virginia (us-east-1)
    - AWS Local Zones: Boston, Chicago, Dallas, Houston, Miami, etc.

## Global Applications Architecture
- Single region, Single AZ
    - No high availability
    - Not good global latency
    - Easy to setup
- Single Region, Multi AZ
    - Has high availability
    - Not good global latency
    - Medium setup difficulty
- Multi region, Active-Passive
    - Has high availability
    - Good global read latency
    - Not good global write latency
    - Medium setup difficulty
- Multi region, Active-Active
    - Has high availability
    - good global read and write latency
    - Difficult to setup

## Summary
- Global DNS: Route 53
    - Great to route users to the closest deployment with the least latency
    - Great for disaster recovery
- Global Content Delivery Network (CDN): CloudFront
    - Replicate part of your application to AWS Edge locations - decrease latency
    - Cache common requests - improved user experience and decreased latency
- S3 Transfer Acceleration
    - Accelerate global uploads and downloads into Amazon S3
- AWS Global Accelerator
    - Improve global application availability and performance using the AWS global network
- AWS Outposts
    - Deploy outpost racks in your own Data Centers to extend AWS services
- AWS Wavelength
    - Brings AWS services to the edge of the 5G networks
    - Ultra-low latency applications
- AWS Local Zones
    - Bring AWS resources closer to your users (Like cities)
    - Good for latency-sensitive applications