# Section 8: Amazon S3

## Overview
- S3 is one of the main building blocks of AWS
- advertised as "infinitely scaling" storage
- Many websites use Amazon S3 as a backbone

## Use Cases
- Backup and Storage
- Disaster Recovery
- Archive
- Hybrid Cloud Storage
- Application hosting
- Media hosting
- Data lakes & big data analytics
- Software delivery
- Static website

## Buckets
- S3 allows people to store objects (files) in "buckets" (directories)
- Buckets are defined at the region level
- S3 looks like a global service but buckets are created in a region
- Naming:
    - (OLD) Shared Global Namespace - have a globally unique name (only one user could have a bucket with a specific name)
    - (NEW) Account Regional Namespace - allows for "reuse" of the same bucket name across regions 
- Naming Constraints:
    - No uppercase
    - No underscore
    - Not an IP
    - Must start with a lowercase letter or number
    - Must NOT start with the prefix xn--
    - Must NOT end with suffix -s3alias

## Objects
- Objects (files) have a key
- The key is the FULL path:
    - s3://my-bucket/my_file.txt
    - s3://my-bucket/my_folder/my_file.txt
- The key is composed of a prefix + object name
    - my_file.txt is the object name
    - my_folder is the prefix
- There is no concept of 'directories' within buckets
    - the ui and key naming scheme make it look like directories
- Object values are the content of the body:
    - Max object size is 50TB
    - If uploading more than 5GB, must use "multi-part upload"
- Metadata (list of text key/value pairs - system or user metadata)
- Tags (unicode key/value pair - up to 10) - useful for security / lifecylce
- Version ID (only if versioning is enabled)

## Security
- User-Based
    - IAM Policies - which API calls should be allowed for a specific IAM user
- Resource-Based
    - Bucket Policies: bucket wide rules from the S3 consol - allows cross account access
    - Object Access Control List (ACL) - finer grain (can be disabled)
    - Bucket Access Control List (ACL) - less common (can be disabled)
- An IAM principal can access an S3 object IF:
    - (The user IAM permissions ALLOW it OR the resource policy ALLOWS it) AND there's no explicit DENY
- Encryption: encrypt object in Amazon S3 using encryption keys

## Bucket Policies
- Json based policies
    - Resources: buckets and objects the policy applies to
    - Effect: Allow / Deny
    - Actions: Set of APIs to allow or deny
    - Principal: The account or user to apply the policy to
- Uses for S3 Bucket policy
    - Grant public access to the bucket
    - Force object to be encrypted at upload
    - Grant access to another account (Cross Account)

## Example Bucket Policy
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "Statement1",
      "Effect": "Allow",
      "Principal": "*",
      "Action": [
        "s3:GetObject"
      ],
      "Resource": "arn:aws:s3:::holm-demo-s3-v1/*"
    }
  ]
}
```

## S3 - Static Website Hosting
- S3 can host static websites and have them accessible on the Internet
- Requires public bucket access if you want everyone to have access
- The website URL will be (depending on region)
    - http://[bucket-name].s3-website-[aws-region].amazonaws.com
    
        OR

    - http://[bucket-name].s3-website.[aws-region].amazonaws.com

## Versioning
- You can version your files in Amazon S3
- It is enabled at the bucket level
- It is best practice to version your buckets
    - Protects agains unintended deletes (ability to restore to a previous version)
    - Easy to roll back to previous version
- Notes:
    - Any file that is not versioned prior to enabling versioning will have version "null"
    - suspending versioning does not delete the previous versions

## Replication
- Versioning MUST be enabled
- Buckets can be in different AWS accounts
- Copying is asynchronous
- Must give proper IAM permissions to the S3 service

### Replication Types:
- Cross region replication (CRR)
    - Used for compliance, lower latency access, replication across accounts
- Same region replication (SRR)
    - Used for log aggregation, live replication between production and test accounts


## Storage Classes

- Durability:
    - Represents how many times an object is going to be lost by S3
    - High durability (99.999999999%, eleven 9's) of objects across multiple AZ
    - If you store 10,000,0000 objects with Amazon S3, you can on average expect to incur a loss of a single object once every 10,000 years
    - Same for all storage classes

- Availability:
    - Measures how readily available a service is
    - Varies depending on storage class
    - Example: S3 standard has 99.99% availability = not available 53 minutes a year

### S3 Standard - General Purpose
- 99.99% Availability
- Used for frequently accessed data
- Low latency and high throughput
- Sustain 2 concurrent facility failures
- Use cases:
    - Big data analytics
    - mobile gaming & applications
    - content distribution

### S3 Standard Infrequent Access (S3 Standard-IA)
- For data that is less frequently accessed, but requires rapid access when needed
- Lower cost that S3 Standard
- 99.9% Availability
- Use cases:
    - Disaster recovery
    - backups
- Amazon S3 One Zone - Infrequent Access (S3 One Zone-IA)
    - High durability (99.999999999%) in a single AZ
    - data lost when AZ is destroyed
    - 99.5% Availability
    - Use Cases:
        - Storing secondary backup copies of on-premise data, or data you can recreate

### S3 Glacier
- Low-cost object storage meant for archiving / backup
- price for storage and price for object retrieval
- S3 Glacier Instant Retrieval
    - Millisecond retrieval, great for data accessed once a quarter
    - Minimum storage duration of 90 days
- S3 Glacier Flexible Retrieval
    - Expedited (1 to 5 minutes)
    - Standard (3 to 5 hours)
    - Bulk (5 to 12 hours) - free
    - Minimum storage duration = 90 days
- S3 Glacier Deep Archive
    - long term storage
    - Standard (12 hours)
    - Bulk (48 hours)
    - Minimum storage duration of 180 days

### S3 Intelligent-Tiering
- Small monthly monitoring and auto-tiering fee
- Moves objects automatically between Access Tiers based on usage
- No retrieval charges
- Frequent Access tier
    - default tier
- Infrequent Access tier
    - objects not accessed for 30 days
- Archive Instant Access tier
    - objects not accessed for 90 days
- Archive Access tier (optional)
    - configurable from 90 days to 700+ days
- Deep Archive Access tier (optional)
    - config from 180 days to 700+ days

### S3 Express One Zone
- High performance, single Availability Zone storage
- Objects stored in a Directory Bucket (bucket in a single AZ)
- Handles 100,000s requests per second with single-digit millisecond latency
- Up to 10x better performance than S3 Standard (50% lower costs)
- High Durability (99.999999999%) and Availability (99.95%)
- Co-locate your storage and computer resources in the same AZ
- Use cases: 
    - latency-sensitive apps
    - data-intensive apps
    - AI & ML training
    - financial modeling
    - media processing
    - HPC
- Best integrated with:
    - SageMaker Model Training
    - Athena
    - EMR
    - Glue

## S3 Encryption

- Service Side Encryption
    - Server handles the encryption after receiving the file
    - Always on by default

- Client Side Encryption
    - User or Application encrypts the file before uploading to S3

## IAM Access Analyzer for S3
- Ensures that only the intended people have access to your S3 bucket
- Evaluates S3 bucket policies, S3 ACLs, S3 Access Point Policies

## AWS Snowball
- Highly Secure, portable devices to collect and process data at the edge, and migrate data into and out of AWS
- Types
    - Storage Optimized - Has more storage
    - Compute Optimized - Has less storage
- AWS Snowball Edge is no longer available to new customers. New customers should explore AWS DataSync for online transfers, AWS Data Transfer Terminal for secure physical transfers, or AWS Partner solutions. For edge computing, explore AWS Outposts. Learn more about your options.

### Data Migration Use Case
- You receive a physical device from AWS to load data onto, once data is loaded the physical snowball device is shipped back to AWS and connected to your AWS infastructure. From there you can load it onto an S3 bucket or some other AWS storage.
- Helps migrate up to Petabytes of data
- If migration will take more than 1 week to transfer all data it is recommended to use a snowball device

### Edge Computing Use Case
- Run EC2 Instances or Lambda function at the edge
- Processing data while it is being created on an edge location
    - a truck on the road
    - a ship at sea
    - an underground mining station
    - location with little or no internet access/compute power
- Device can be reconnected to AWS at a later time

### Snowball pricing
- pay for device usage and data transfer out of AWS
    - Data transfer in is free
- On-Demand
    - one-time service fee per job which includes
        - 10 days of usage for snowball edge Storage Optimized 80TB
        - 15 days of usage for snowball edge Storage Optimized 210TB
    - shipping days are not counted towards the included days
    - charged per day for any additional days
- Committed Upfront
    - Pay in advace for monthly, 1-year, and 3-years of usage
    - Up to 62% discounted pricing

## Storage Gateway (Hybrid Cloud)
- Allows on-premise systems to seamlessly use the AWS Cloud storage
- Hybrid Cloud
    - Part of infrastructure is on-premise
    - Part of you infrastructure is on the cloud
- Reasons to do hybrid cloud
    - Long cloud migrations
    - security requirements
    - compliance requirements
    - IT strategy

## S3 Summary
- Buckets vs Objects
    - Buckets have a gloabl unique name and are tied to a region
    - Objects live within buckets (like files)
- S3 Security
    - IAM policies
    - S3 bucker policies
    - S3 encryption
- S3 Websites
    - host static website on Amazon S3
- S3 Versioning
    - multiple versions for files, prevents accidental deletes
- S3 replication
    - same region or cross-region, must enable versioning
- S3 Storage Classes
    - Standard
    - IA
    - One Zone IA
    - Intelligent
    - Glacier (Instant, Flexible, Depp)
- Snowball
    - import/export data in S3 througha physical device
    - edge computing
- Storage Gateway
    - enables hybrid cloud by bridging on-premise storage to S3



## Shared Responsibilty Model

### AWS
- Infrastructure
- Configuration and vulnerability analysis
- Compliance Validation

### User
- S3 Versioning
- S3 Bucket Policies
- S3 Replication setup
- Logging and Monitoring
- Picking S3 Ctorage Classes
- Data encryption at rest and in transit
