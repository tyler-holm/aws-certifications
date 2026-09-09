# Section 16: Security & Compliance

## AWS Shared Responsibility Model
- AWS responsibility - Security of the cloud
    - Protecting Infrastructure (hardware, software, facilities, and networking) that runs all the AWS service
    - Managed services like S3, DynamoDB, RDS, etc
- Customer responsibility - security in the cloud
    - For EC2 instance, customer is responsible for management of the guest OS, firewall and network configuration, IAM roles and users
    - Encrypting applicaiton data
- Shared controls
    - Patch management, Configuration Management, Awareness and Training
- Example: RDS
    - AWS responsibility:
        - manage the underlying EC2 instance, disable SSH access
        - automate DB patching
        - automate DB patching
        - audit the underlying instance and disks & guarantee it functions
    - Customer Responsibility:
        - Check the ports / IP / security group inbouf rules in DB's security group
        - creating a database with or without public access
        - ensure parameter groups or DB is configured to only allow SSL connections
        - Database encryption settings
- Example: S3
    - AWS responsibility:
        - guarantee you get unlimited storage
        - guarantee you get encryption
        - ensure seperation of the data between different customers
        - ensure AWS employees can't access your data
    - Customer Responsibility:
        - Bucket configuration
        - bucket policy / public setting
        - IAM users and roles
        - enabling encryption

## DDOS Attack Protection
- Distributed denial of service 

### AWS Sheild 
- Sheild Standard: 
    - protects against DDOS attacks for your website and applications, 
    - Free service that is activated for every AWS customer
    - Protects agains SYN/UPD Floods, Reflection attacks, and other layer 3/layer 4 attacks
- Sheild Advanced: 
    - 24/7 premium DDOS protection
    - Optional DDoS mitigation service ($3,000 per month per organization)
    - Protect agains more sophisticated attacks on Amazon EC2, Elastic Load Balancing, Amazon CloudFront, AWS Global Accelerator, and Route 53
    - 24/7 access to AWS DDoS response team (DRP)
    - Protect against higher fees during usage spikes due to DDoS

### AWS WAF
- Web application firewall
- filter specific requests based on rules 
- Protects your web applications from common web exploits (Layer 7 - HTTP)
- Deploy on Application Load Balancer, API Gateway, CloudFront
- Define WEB ACL
    - Rules can include IP addresses, HTTP headers, HTTP body, or URI strings
    - Protects from common attacks - SQL injection and XSS
    - Size constraints, geo-match (block countries)
    - Rate-based rules (to count occurences of events) - for DDoS protection
### CloudFront and Route 53
- Availabilty protection using global edge network
- Combined with AWS Shield, provides attack mitigation at the edge

### AWS Auto Scaling
- leverage scaling to handle attacks more smoothly

## AWS Network Firewall
- protects your entire VPC all at once 
- from Layer 3 to Layer 7 protection
- Can inspect traffic in any direction


## AWS Firewall Manager
- EXAM NOTE: managing VPC security group accross multipel account in an organization = Firewall Manager 
- Manage security rules in all accounts of an AWS Organization
- Security policy: common set of security rules
    - VPC security groups for EC2, Application Load Balancer, etc
    - WAF rules
    - AWS Shield Advanced
    - AWS network firewall
- Rules are applied to new resources as they are created (good for compliance) accross all existing and future accounts in your organization

## Penetration Testing
### Allowed Activities
- EC2 instances, NAT gateways, and ELB
- Amazon RDS
- CloudFront
- Aurora
- API Gateways
- Lambda and Lambda Edge functions
- Lightsail resources
- Elaastic Beanstalk

### Prohibited Activities
- DNS zone walking via Route 53 Hosted Zones
- DDoS attacks
- Port flooding
- Protocol flooding
- Request flooding (login request flooding, API request flooding)

### Others
- For other simulated events, contact the aws security team
- aws-security-simulated-event@amazon.com

## Encryption
- Data at rest: data is stored or archived on a device
- Data in transit: Data being moved over the network from one place to another
- Ideally we want data encrypted in both places

### AWS Key Management Service (KMS)
- EXAM NOTE: encryption for an AWS service, its most likely KMS
- in KMS AWS manages the encryption keys for us
- Encryption opt-in:
    - EBS volumes: encrypt volumes
    - S3 buckets: server-side encryption of objects (SSE-S3 enabled by default, SSE-KMS opt in)
    - Redshift database: encryption of data
    - RDS database: encryption of data
    - EFS drives: encryption of data
- Encryption Automatically Enabled:
    - CloudTrail Logs
    - S3 Glacier
    - Storage Gateway

### CloudHSM
- AWS provisions encryption hardware
- Dedicated hardware (HSM = Hardware Security Module)
- You mananage your own encryption keys (not AWS)
- HSM device is tamper resistant, FIPS 140-2 Level 3 compliance

### Types of KSM keys
- Customer Managed Key:
    - Created, managed, and used by the customer.
    - Can enable or disable
    - Possibility of rotation policy
    - Possibility to bring your own key
- AWS managed Key:
    - Created, managed, and used on the customer's behalf by AWS
    - Used by AWS services (names have "aws/" prefix)
- AWS Owned keys
    - Collection of CMKs that an AWS service owns and manages to use in multiple accounts
    - AWS can use these to protect resources in your account, but you can't view them
- CloudHSM Keys
    - keys generated from your own CloudHSM hardware device
    - Cryptographic operation are performed within the CloudHSM cluster

## AWS Certificate Manager (ACM)
- EXAM NOTE: SSL/TLS certificate = ACM
- Easily provision, manage, and deploy SSL/TLS certificates
- Used to provide in-flight encryption for websites (HTTPS)
- Supports public and private TLS certificates
- Free of charge for public TLS certificates
- Automatic TLS certificate renewal
- Integrations with (load TLS certificates on)
    - Elastic Load Balancer
    - CloudFront Distributions
    - APIs on API gateway

## AWS Secret Manager
- newer service for storing secrets
- capability to force rotation of secrets every X days
- Automate generation of secrets on rotation (uses Lambda)
- Integration with RDS (MySQL, PostgreSQL, Aurora)
- Secrets are encrypted with KMS
- Mostly meant for RDS integration
