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

