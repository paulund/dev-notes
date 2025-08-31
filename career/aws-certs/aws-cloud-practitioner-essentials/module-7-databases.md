# Module 7: Databases

## Introduction To Databases

AWS offers database services for:

- Relational databases
- Non-relational databases
- In-memory databases
- Purpose built databases

## Amazon Relational Database Service (RDS)

Amazon RDS is a managed relational database service that supports several database engines, including:

- Amazon Aurora
- PostgreSQL
- MySQL
- MariaDB
- Oracle
- Microsoft SQL Server

RDS automates time-consuming tasks such as hardware provisioning, database setup, patching, and backups.

Relational databases are rigid schema that organises collections of data into tables with rows and columns.

## Amazon Aurora

Amazon Aurora is a MySQL and PostgreSQL-compatible relational database built for the cloud, offering high performance and availability. It automatically scales storage up to 128 TB per database instance and provides replication across multiple Availability Zones for fault tolerance.

Aurora replicates data across multiple Availability Zones to enhance durability and availability.

Offers 5 times the performance of standard MySQL databases and 3 times that of standard PostgreSQL databases.

## NoSQL Database Services

AWS offers several NoSQL database services, including:

- Amazon DynamoDB
- Amazon DocumentDB
- Amazon ElastiCache

These services are designed to handle unstructured and semi-structured data, providing flexibility and scalability for modern applications.

### DynamoDB

Serverless NoSQL database that provides single-digit millisecond latency at any scale.

This is good for applications that don't have a well defined schema.

## In-Memory Databases

AWS offers in-memory database services such as Amazon ElastiCache and Amazon MemoryDB for Redis, which provide low-latency data access for caching and real-time analytics.

Caching services offer sub-millisecond latency for fast data retrieval, making them ideal for use cases such as session management, gaming leaderboards, and real-time analytics.

## Amazon DocumentDB

Amazon DocumentDB is a fully managed document database service that supports MongoDB workloads. It is designed for scalability, performance, and availability, making it suitable for modern applications that require flexible data models.

JSON-like document structure allows for easy storage and retrieval of semi-structured data, making it a good fit for applications that need to evolve their data models over time.

## Amazon Neptune

Amazon Neptune is a fully managed graph database service that supports both property graph and RDF graph models. It is designed for use cases such as social networking, recommendation engines, and fraud detection, where relationships between data points are critical.

## Amazon Managed Blockchain

Amazon Managed Blockchain is a fully managed service that makes it easy to create and manage scalable blockchain networks using popular open-source frameworks such as Hyperledger Fabric and Ethereum. It simplifies the setup and management of blockchain networks, allowing developers to focus on building their applications.

## AWS Backup

AWS Backup is a fully managed backup service that makes it easy to centralize and automate the backup of data across AWS services. It provides a consistent backup strategy for AWS resources, helping organizations meet their compliance and regulatory requirements.
