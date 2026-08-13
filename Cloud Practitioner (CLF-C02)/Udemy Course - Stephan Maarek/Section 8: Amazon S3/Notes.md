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





