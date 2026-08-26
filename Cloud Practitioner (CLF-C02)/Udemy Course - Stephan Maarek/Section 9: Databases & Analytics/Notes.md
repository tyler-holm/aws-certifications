# Section 9: Databases & Analytics

## Types of Databases Overview

###  Relational Databases
- Looks just like excel spreadsheets, with links between them
- Uses SQL to preform queries / lookups

### NoSQL Database
- NoSQL = non-SQL = non relational databases
- NoSQL databases are purpose built for specific data models and have flexible schemas for building modern applications
- Benefits
    - Felxibility: easy to evolve data model
    - Scalability: designed to scale-out by using distributed clusters
    - High performance: optimized for case-specific data model
    - Highly functional: types optimized for the data model
- Examples: Key-value, document, graph, in-memory, search databases

#### NoSQL data example: JSON
- JSON = JavaScript Object Notation
- JSON is a common form of data that fits into a NoSQL model
- Data can be nested
- Fields can change over time
- Support for new types: arrays, etc

## Databases and Shared Responsibility Model
- AWS offers use to manage different databases
- Benefits include 
    - Quick provisioning, high availability, vertical and horizontal scaling
    - Automated backup and restore, operation, upgrades
    - Monitoring, alerting
- Note: many databases technologies could be run on EC2 but you must handle yourself the resiliency, backup, patching, high availability, fault tolerance, scaling...


## Amazon RDS
- RDS (relational database service)
- It's a managed DB service for DB using SQL as a query language
- It allows you to create databases in the cloud that are managed by AWS
- Supports the following:
    - Postgras
    - MySQL
    - MariaDB
    - Oracle
    - Microsoft SQL Server
    - IBM DB2
    - Aurora (AWS Propietary database)

### Pros and Cons of Amazon RDS
- Benefits of RDS over deploying a DB on EC2
    - Managed service
    - Automated provisionsing, OS patching
    - Continuous backups and restore to specific timestamp
    - mointoring dashboards
    - read replicas for improved read performance
    - Multi AZ setup for disaster recovery
    - maintenance windows for upgrades
    - Scaling capability (vertical and horizontal)
    - Storage backed by EBS
- Cons of RDS over deploying a DB on EC2
    - Can't SSH into your instances

### RDS Deployements

#### Read Replicas
- Replica of your main database
- Can create up to 15 Read Replicas
- Increases read capacity
- Writes all still happen on the main database

#### Multi-AZ
- Failover incase of AZ outage (high availability)
- Read and Writes happen on the main database
- Failover DB only becomes active if the main database goes down

#### Multi-Region
- Read replicas across multiple regions
- All writes still happen on a single main database
- Data is replicated to databases in other regions for read only
- Good for disaster recovery
- improves read performance in multiple regions
- network costs associated with replicating data

## Amazon Aurora
- Aurora is a propietary technology from AWS
- Postgre and MySQL are both supported as Aurora DB
- Aurora is AWS cloud optimized and claims 5x performance improvement over MySQL on RDS, over 3x the performance of Postgres on RDS
- Aurora storage automatically grows in increments of 10GB, up to 256TB
- Aurora costs more than RDS (20% more) - but is more efficient

### Aurora Serverless
- Automated database instantiation and auto-scaling based on actual usage
- PostgreSQL and MySQL are both supoorted as Aurora Serverless DB
- No capacity planning needed
- Least management overhead
- Pay per second, can be more cost effective
- Use cases:
    - infrequent, intermittent, or unpreditable workloads

## DocumentDB
- similar to Aurora, but for MongoDB (NoSQL database)
- Used to store, query, and index JSON data
- Fully managed, highly available with replication across 3 AZs
- DocumentDV storage automatically grows in increments of 10GB
- Automatically scales to workloads with millions of requests per second

## Neptune
- Fully managed graph database
- a popular graph dataset would be a social network
    - users have friends
    - posts have comments
    - comments have likes
    - Users share and like posts
- highly available across 3 AZs, with up to 15 read replicas
- build and run applications working with highly connected datasets - optimized for these complex and hard queries
- can store up to billions of relations and query the graph with millisecond latency
- Great for knowledge graphs (wikipedia), fraud detection, recommendation engines, social networking
- EXAM NOTE: graph database = use neptune

## Timestream
- Fully managed, fast, scalable, serverless, time series database
    - data that is evolving over time
- Automatically scales up/down to adjust capacity
- Stroe and analyze trillions of events per day
- 1000s time faster and 1/10 the cost of relational databases
- Built-in time series analytics functions (helps you identify patterns in you data in near real-time)

## Amazon ElastiCache

### Overview
- get managed Redis or Memcached database
- in memory databases with high-performance, low-latency
- Helps reduce load off of RDS databases for read intensive workloads
- Aws takes care of OS maintenance / patching, optimizations, setup, configuration, monitoring, failure recovery and backups

## DynamoDB
- Fully managed highly available database with replication across 3 AZs
- NoSQL database
- Scales to massive workloads, distributed "serverless" database
- Millions of requests per second, trillions of rows, 100s of TB storage
- Fast and consistent in performance
- Single-digit millisecond latency - low latency retrieval
- Integrated with IAM for security, authorization and administration
- Low cost and auto scaling capabilities
- Standard & Infrequent Access (IA) Table Class

### DynamoDB - Type of Data
- key/value database
- primary key made of Partion Key and Sort Keys
- Values are attributes, not all primary keys need to have the same attributes

### DynamoDB Accelerator - DAX
- Fully managed in-memory cache for DynamoDB
- Caches the most frequently used objects
- 10x performance improvement
- Secure, highly scalable, and highly available
- Similar to ElasticCache but have better integration with DynamoDB

### DynamoDB - Global Tables
- Make a DynamoDB table accessible with low latency in multiple-regions
- Two way replication across regions
- Active-Active replication (read/write to any AWS region)

## Redshift
- redshit database is based on PostgresSQL, but is not used for online transaction processing (OLTP)
- Used for online analytical processing (OLAP) - analytics and data processing
- Load data once every hour, not every second
- 10x better performance than other data warehouses, scale to PBs of data
- Columnar storage of data (instead of row based)
- Massive Parallel Query Execution (MPP), highly available
- Pay as you go based on the instances provisioned
- Has a SQL interface for performing the queries
- BI tools such as AWS Quicksight or Tableau integrated with it


### Redshift serverless
- Automatically provisions and scales data warehouse underlying capacity
- Run analytics workloads without managing data warehouse infrastructure
- Pay only for compute and storage used during analysis
- Use cases: reporting, dashboarding applications, real-time analytics

## EMR
- Elastic MapReduce
- Helps create a Hadoop cluster (Big Data)
    - Hadoop clusters are used to analyze and process vast amounts of data
- clusters can be made of hundreds of EC2 instances that work together
- supports Apache spark, HBase, Presto, Flink, etc.
- EMR provisions and configures all EC2 instances to work together
- Auto scaling
- Used for:
    - data processing
    - machine learning
    - web indexing
    - big data

## Athena
- Serverless query service to perform analytics agains S3 objects
- Uses standard SQL language to query the files
- Supports CSV, JSON, ORC, Avro, and Parquet
- Pricing 
    - $5 per TB of data scanned
    - Use compressed or columnar data for cost-savings (scan less)
- Use cases:
    - Business Intelligence / analytics / reporting
    - analyze and query VPC flow logs
    - ELB logs
    - CloudTrail trails
    - etc.
- EXAM NOTE: analyze data in S3 using serverless SQL = use Athena

## QuickSight
- Serverless machine learning-powered business intelligence service to create interactive dashboards
- Fast automatically scalable, embeddable, with per-session pricing
- Use cases: 
    - Business analytis
    - Building visualizations
    - Perform ad-hoc analysis
    - Get business insights using data
- Integrated with RDS, Aurora, Athena, Redshift, S3, etc.

## Amazon Managed Blockchain
- decentralized blockchain
- Blockchain makes it possible to build applications where multiple parties can execute transactions without the need for a trusted, central authority
- Used to:
    - Join public blockchain networks
    - create your own scalable private network
- Compatible with the frameworks Hyperledger Fabric and Ethereum
- EXAM NOTE: blockchain, Hyperledger Fabric, Ethereum = Amazon Managed Blockchain

## AWS Glue
- Manage extract, transform, and load (ETL) service
- Used to prepare and transform data to analytics
- Fully serverless service
- Data can be extracted from multiple databases, transformed, and loading into another service or database

### Glue Data Catalog
- catalog of datasets across AWS
- can be used by Athena, Redshift, EMR, etc
- used to find and build datasets from other databases

## Database Migration Service (DMS)
- EC2 instance runs DMS
- DMS instance extracts data from source DB
- Loads data into Target DB
- Quick and securely migrate databases to AWS, resilient, self healing
- The source database remain available during migration
- Supports: 
    - Homogeneous migration (ex. Oracle to Oracle)
    - Heterogenious migration (ex. Microsoft SQL server to Aurora)
- EXAM NOTE: Database migration = use DMS

## Databases & Analytics Summary
- Relational Databases - OLTP: RDS and Aurora (SQL)
- Difference between Multi-AZ, Read Replicas, Multi-Region
- In-memory database (cache): Elasticache
- Key/Value Database: DynamoDB (serverless) and DAX (cache for DynamoDB)
- Warehouse - OLAP: Redshift (SQL)
- Hadoop Cluster: EMR
- Athena: query data on Amazon S3 (serverless and SQL)
- Quicksight: dashboards on your data (serverless)
- DocumentDB: Aurora for MongoDB (JSON - NoSQL database)
- Amazon Managed Blockchain: managed Hyperledger Fabric & Ethereum blockchains
- Glue: Managed ETL (Extract Transform Load) and Data Catalog Server
- Database Migration: DMS
- Neptune: graph database
- Timestream: Time-series database


