# Section 16: Security & Compliance

## AWS Shared Responsibility Model
- AWS responsibility - Security of the cloud
    - Protecting Infrastructure (hardware, software, facilities, and networking) that runs all the AWS service
    - Managed services like S3, DynamoDB, RDS, etc
- Customer responsibility - security in the cloud
    - For EC2 instance, customer is responsible for management of the guest OS, firewall and network configuration, IAM roles and users
    - Encrypting application data
- Shared controls
    - Patch management, Configuration Management, Awareness and Training
- Example: RDS
    - AWS responsibility:
        - manage the underlying EC2 instance, disable SSH access
        - automate DB patching
        - automate DB patching
        - audit the underlying instance and disks & guarantee it functions
    - Customer Responsibility:
        - Check the ports / IP / security group inbound rules in DB's security group
        - creating a database with or without public access
        - ensure parameter groups or DB is configured to only allow SSL connections
        - Database encryption settings
- Example: S3
    - AWS responsibility:
        - guarantee you get unlimited storage
        - guarantee you get encryption
        - ensure separation of the data between different customers
        - ensure AWS employees can't access your data
    - Customer Responsibility:
        - Bucket configuration
        - bucket policy / public setting
        - IAM users and roles
        - enabling encryption

## DDOS Attack Protection
- Distributed denial of service 

### AWS shield 
- shield Standard: 
    - protects against DDOS attacks for your website and applications, 
    - Free service that is activated for every AWS customer
    - Protects against SYN/UPD Floods, Reflection attacks, and other layer 3/layer 4 attacks
- shield Advanced: 
    - 24/7 premium DDOS protection
    - Optional DDoS mitigation service ($3,000 per month per organization)
    - Protect against more sophisticated attacks on Amazon EC2, Elastic Load Balancing, Amazon CloudFront, AWS Global Accelerator, and Route 53
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
    - Rate-based rules (to count occurrences of events) - for DDoS protection
### CloudFront and Route 53
- availability protection using global edge network
- Combined with AWS Shield, provides attack mitigation at the edge

### AWS Auto Scaling
- leverage scaling to handle attacks more smoothly

## AWS Network Firewall
- protects your entire VPC all at once 
- from Layer 3 to Layer 7 protection
- Can inspect traffic in any direction


## AWS Firewall Manager
- EXAM NOTE: managing VPC security group across multiple account in an organization = Firewall Manager 
- Manage security rules in all accounts of an AWS Organization
- Security policy: common set of security rules
    - VPC security groups for EC2, Application Load Balancer, etc
    - WAF rules
    - AWS Shield Advanced
    - AWS network firewall
- Rules are applied to new resources as they are created (good for compliance) across all existing and future accounts in your organization

## Penetration Testing
### Allowed Activities
- EC2 instances, NAT gateways, and ELB
- Amazon RDS
- CloudFront
- Aurora
- API Gateways
- Lambda and Lambda Edge functions
- Lightsail resources
- Elastic Beanstalk

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
- You manage your own encryption keys (not AWS)
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
- Integration with RDS (MySQL, postgresQL, Aurora)
- Secrets are encrypted with KMS
- Mostly meant for RDS integration

## AWS Artifact
- Portal that provides customers with on-demand access to AWS compliance documentation and AWS agreements
- Artifact Reports 
    - Allows you to download AWS security and compliance documents from third-party auditors, like AWS ISO certifications, Payment Card Industry (PCI), and System and Organization Control (SOC) reports
- Artifact Agreements
    - Allows you to review, accept, and track the status of AWS agreements such as the Business Association Addendum (BAA) or the Health Insurance Portability and Accountability Act (HIPAA) for an individual account or in your organization
- Can be used to support internal audits or compliance 

## Amazon GuardDuty
- Intelligent Threat discovery to protect your AWS Account
- Uses Machine Learning algorithms, anomaly detection, 3rd party data
- One click to enable (30 day trial), no need to install software
- Input data includes:
    - CloudTrail Event Logs 
        - Unusual API calls, unauthorized deployments
        - CloudTrail Management Events 
            - Create VPC subnet, create trail, etc.
        - CloudTrail S3 Data Events 
            - get object, list objects, delete objects, etc.
        - VPC Flow Logs
            - Unusual internal traffic, unusual IP address
        - DNS Logs
            - compromised EC2 instances sending encoded data within DNS queries
        - Optional Features
            - EKS Audit Logs, RDS & Aurora, EBS, Lambda, S3 Data Events, etc.
- Can setup EventBridge rules to be notified in case of findings
- EventBridge rules can target AWS Lambda or SNS
- Can protect against CryptoCurrency attacks (has a dedicated "finding" for it)

## AWS Inspector
- Automated Security Assessments
- For:
    - ECS instances
        - Leverages the AWS System Manager (SSM) agent
        - Analyze against unintended network accessibility
        - Analyze the running OS against known vulnerabilities
    - For Container Images pushed to Amazon ECR
        - Assessment of Container Images as that are pushed
    - For Lambda Functions
        - Identifies software vulnerabilities in function code and package dependencies
        - Assessment of functions as they are deployed
    - Reporting and integration with AWS Security Hub
    - Send findings to Amazon Event Bridge
- Only for EC2 Instances, Container Images, and Lambda Functions
- Continuous scanning of infrastructure as needed
- Package vulnerabilities (EC2, ECR, and Lambda) - database of CVEs
- Network reachability (EC2)
- A risk score is associated with all the vulnerabilities for prioritization

## AWS Config
- Helps with auditing and recording compliance of your AWS resources
- Helps record configurations and changes over time
- Possibility of storing the configuration data into S3 (Analyzed by Athena)
- Questions that can be solved by AWS Config:
    - Is there unrestricted SSH access to my security groups?
    - Do my buckets have any public access?
    - How has my ALB configuration changed over time?
- You can receive alerts (SNS notifications) for any changes
- AWS Config is a per-region service, but all configs can be aggregated across regions and services
- Resources will show a history of the config and if the configuration was compliant at a specific point in time

## AWS Macie
- fully managed data security and data privacy service that uses machine learning and pattern matching to discover and protect your sensitive data in AWS
- helps identify and alert you to sensitive data, such as personally identifiable information (PII)

## AWS Security Hub
- Central security tool used to manage security across several AWS accounts and automate security checks
- integrated dashboards showing current security and compliance status to quickly take actions
- Automatically aggregates alerts in predefined or personal findings formats from various AWS services & AWS partner tools:
    - Config
    - GuardDuty
    - Inspector
    - Macie
    - IAM Access Analyzer
    - AWS Systems Manager
    - AWS Firewall Manager
    - AWS health
    - AWS Partner Network Solutions
- Must first enable the AWS Config Service
- Integrates with Amazon Detective to discover where issues are coming from

## Amazon Detective
- Sometimes you require deeper analysis to isolate WHY an vulnerability or security issue happened
- analyzes, investigates, and quickly identifies the root cause of security issues or suspicious activities (using ML and graphs)
- Automatically collects and processes events fromVPC Flow Logs, CloudTrail, GuardDuty, and creates a unified view
- Produces visualization with details and context to help get to the root cause of an issue

## AWS Abuse
- Report suspected AWS resources for abusive or illegal purposes
- Abusive and prohibited behaviors are:
    - Spam 
        - receiving undesired emails from AWS-owned IP address
        - websites, and forums spammed by AWS resources
    - Port scanning - sending packets to your ports to discover unsecured ones
    - DoS or DDoS attacks 
        - AWS owned IP addresses attempting to overwhelm or crash servers/softwares
    - Intrusion attempts - logging in on unauthorized resources
    - Hosting objectionable or copyright content
        - distributing illegal or copyrighted content without consent
    - Distributing Malware - AWS resources distributing softwares to harm computers or machines
- contact the AWS Abuse team via the AWS abuse form or abuse@amazonaws.com

## Root User Privileges
- Root user = Account Owner (created when the account is created)
- Has complete access to all AWS services and resources
- Lock away your AWS account root user access keys
- DO NOT use the root account for everyday tasks, even admin tasks
- Actions that can only be performed by the root user
    - Change account settings (account name, email address, root user password, root user access keys)
    - View certain tax invoices
    - Close an AWS account
    - Restore IAM user permissions
    - Change or cancel your AWS support plan
    - Register as a seller in the reserved instance marketplace
    - Configure an Amazon S3 bucket policy that includes an invalid VPC ID or VPC endpoint ID
    - Sign up for GovCloud

## IAM Access Analyzer
- Find out which resources are shared externally
    - S3 buckets
    - IAM roles
    - KMS Keys
    - Lambda Functions and Layers
    - SQS queues
    - Secrets Manager Secrets
- Define Zone of Trust (AWS account or AWS Organization)
- Access outside zone of trust is flagged as findings

## Summary
- Shared Responsibility Model
- Shield: Automatic DDoS Protection 
    - 24/7 support if you upgrade to advanced
- WAF: Firewall to filter incoming requests based on rules
- KMS: Encryption keys managed by AWS
- CloudHSM: Hardware encryption, we manage encryption keys
- AWS Certificate Manager: provision, manage, and deploy SSL/TLS Certificates
- Artifact: Get access to compliance reports such as PCI, ISO, etc...
- GuardDuty: Find malicious behavior with VPC, DNS, and CloudTrail logs
- Inspector: find software vulnerability in EC2, ECR Images, and Lambda functions
- Network Firewall: protects VPC against network attacks
- Config Track config changes and check compliance against rules
- Macie: Find sensitive data (ex: PII data) in S3 buckets
- CloudTrail: Track API calls made by users within the account
- AWS Security Hub: gather security findings from multiple AWS accounts
- Amazon Detective: find the root cause of security issues or suspicious activities
- AWS Abuse: Report AWS resources used or abuse of illegal purposes
- Root user privileges:
    - Change account settings
    - Close Account
    - Change or cancel AWS support plan
    - Register as a seller in the Reserved Instance Marketplace
- IAM Access Analyzer: identify which resources are shared externally
- Firewall Manager: manage security rules across an organization (WAF, Shield, etc.)

