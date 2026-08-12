# Section 6: EC2 Instance Storage

## EBS Overview

EBS = Elastic Block Store

- Network drive you can attach to instances while they run
- Allows persistant data, even after instance terminations
- Instances can have more than one EBS mounted, but an EBS can only be mounted to one instance at a time
- Can be attached on demand
- Bound to one availability zone at a time
- To move to a new az, you need to create a snapshot
- Communicating over the network can create latency
- Can be detached from one instanc and reatached to another instance (good for failover)
- GBs and IOPS must be provisioned in advance

## EBS Snapshots

- Makes a backup of your EBS volume at that point in time
- Recommended to detach a volume before doing a snapshot, but not required
- Allows copying snapshots across AZ or region

### Features

- EBS Snapshot Archive
    - Move a snapshot to an "archive tier" this is 75% cheaper
    - Archived snapshots can take 24 to 72 hours to restore
    - Best for archived that are not used often (very old backups)

- Recyle Bin for EBS Snapshots
    - Setup rules to retain deleted snapshots for a specified time frame (1 day to 1 year)
    - Snapshots are permanently deleted after the specified time frame
    - Allows recovering deleted snapshots

## AMI Overview

AMI = Amazon Machine Image

- AMIs are a customization of an EC2 instance.
    - add your own software, config, os, monitoring, etc
    - Faster boot / configuration time because all software is pre-packaged
- AMIs are built for a specific region (can be copied to a new region)
- EC2 instances can be launched from an AMI
    - Public AMI: AWS provided
    - Custom AMI: you make and maintain them
    - Marketplace AMI: an AMI someone else made (potentially costs money)

### AMI Manual Creation Process
1. Start and EC2 instance and customize it
2. Stop the instance (for data integrity)
3. Build an AMI from the instance (also creates an EBS snapshot)
4. Launch new instances from the built AMI

### EC2 Image Builder
- Automates the creation, maintenance, validation, and testing of EC2 AMIs
- Can be run on a schedule (weekly, when package updates are found, etc)
- Free service

1. EC2 image builder service runs
2. EC2 image builder launches a Builder EC2 instance
3. Builder EC2 instance installs updates, custom configurations, etc.
4. A new AMI is created from the Builder EC2 instance
5. A Test EC2 Instance is launched
6. User defined tests run to validate the AMI
7. AMI is distributed

## EC2 Instance Store
- hardware disk
- better I/O performance than EBS
- can only attach to one instance at a time
- EC2 isntance stores lose their storage if they're stopped (ephemeral)
- Good for buffer / cache / scratch data / temporary content
- Bad for long term storage
- Risk of data lose if hardware fails
- Backups and replication are your responsibility


## EFS - Elastic File System
- Managed Network file system (NFS)
- Can be mounted to 100s of EC2s at a time
- Works only with Linux EC2 instances
- works across multiple AZs
- Highly available, scalable, expensive, pay per use, no capacity planning

### EFS-IA - Infrequent Access
- Storage class is cost optimized for files not accessed every day
- Up to 92% lower cost compared to EFS standard
- If enabled, EFS will automatically move your files to EFS-IA based on the last time they were accessed
- Enable EFS-IA with a specific lifecyle policy (example: 60 days)
- Applications accessing EFS don't need to do anything differently for EFS-IA

## Amazon FSx
### Overview
- Launch 3rd party high-performance file systems on AWS
- Fully managed service

### Amazon FSx for Windows File Server
- Fully managed, highly reliable, and scalable Windows native shared file system
- Built on Windows File Server
- Supports SMB protocol and Windows NTFS
- Integrated with Microsoft AD
- Can be accessed from AWS or your on-premise infrastructure

### Amazon FSx for Lustre
- A fully managed, high-performance, scalable file storage for High Performance
- Linux file system
- "Lustre" is derived from "Linux" and "Cluster"
- Machine Learning, Analytics, Video Processiong, Financial Modeling, etc...
- Scales up to 100s GB/s, millions of IOPS, sub-ms latencies

## Shared Responsibility Model

### AWS
- Infrastructure
- Replication for data for EBS volumes and EFS drives
- Replace faulty hardware
- Ensure their employees cannont access your data (compliance)


### User
- Setting up backup / snapshot
- Setting up data encryption
- Responsibility of any data on the drives
- Understanding the risk of using EC2 Instance Store (hardware failure, corrupt data, etc)





