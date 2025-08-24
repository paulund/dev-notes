# Module 6: Storage

## Introduction To Storage

- There are multiple different options for storage in AWS, depending on needs.
- Block storage, low latency volumes that can be attached to EC2 instances
    - EBS (Elastic Block Store)
    - Best for databases and applications that require consistent and low-latency performance
- Object storage, save data in objects.
    - Data, unique ID and metadata
    - Organised in buckets
    - Best for images and videos
- File storage, a hierarchical storage structure that allows for files to be organized in directories.
- AWS storage gateway - hybrid cloud storage service that provides on-premises access to virtually unlimited cloud storage.
- AWS Elastic disaster recovery - provides fast and reliable recovery of applications and data in the cloud.

## Block storage
- AWS EC2 instance store will not persist independently from the EC2 instance.
- EBS (Elastic Block Store)
- Best for databases and applications that require consistent and low-latency performance
- EBS volumes will persist independently from the EC2 instance and can be attached to your EC2 instance


## Amazon Elastic Block Store Data Lifecycle
- Amazon EBS snapshots
- Amazon EBS data lifecycle
  - Involves creating, backing up and deleting EBS volumes and snapshots
- Customer is responsible for the EBS snapshots and amazon data lifecycle manager
- EBS snapshots are point-in-time copies of EBS volumes and can be scheduled, hourly, daily, weekly, or monthly, using amazon data lifecycle manager.
- EBS snapshots are capable of incremental snapshots, meaning only the changes made since the last snapshot are saved. Making is cheaper and faster.
- Snapshots are used for disaster recovery, data migration and backups.

## Amazon Simple Storage Service (S3)

- S3 is object storage and uses buckets
- Can store unlimited amounts of data in AWS cloud
- Good for documents, images, videos
- Has features like versioning, lifecycle management and different cost optimization options
- Supports multiple storage classes (e.g., S3 Standard, S3 Intelligent-Tiering, S3 Glacier) for different use cases and cost requirements
- Common use case is for hosting static websites
- Everything stored in S3 is private by default

## S3 Storage Classes and Lifecycle

- S3 standard - general purpose used for dynamic websites
- S3 standard infrequent access - used for data that is not accessed frequently
- S3 One Zone-IA - lower-cost option for infrequently accessed data that does not require multiple availability zone resilience
- S3 Express one zone - optimized for low-latency access to data stored in a single availability zone
- S3 Intelligent-Tiering - automatically moves data between two access tiers when access patterns change
- S3 Glacier instant retrieval - low-cost storage for data that is rarely accessed
- S3 Glacier flexible retrieval - low-cost storage for data that is infrequently accessed
- S3 Glacier deep archive - lowest-cost storage for data that is rarely accessed
- S3 Outposts - delivers S3 storage to on-premises environments
- Lifecycles can be used to automatically transition objects between storage classes or delete them after a certain period.

## Amazon Elastic File System (EFS)

- EFS is a managed service for file storage that can be accessed from multiple EC2 instances.
- It provides a scalable and elastic file system that grows and shrinks automatically as you add and remove files.
- EFS is designed for use cases such as content management, web serving, and data sharing.
- EFS can connect cross region
- EFS has lifecycle functionality to automatically move files to different storage classes or delete them after a certain period.

## Amazon FSx

- FSx is a managed service for file storage that provides high-performance file systems for specific use cases.
- FSx supports multiple filesystems, including Windows File Server and Lustre.
- FSx will allow you to migrate windows file servers into the cloud

## AWS Storage Gateway

- Hybrid solution that connects your on-premises infrastructure to cloud storage.
- You can extend your local storage into the cloud using AWS Storage Gateway.

## AWS Elastic Disaster Recovery

- Elastic disaster recovery replicates critical workloads to AWS.
- A use case for this could be a company that needs to ensure business continuity in the event of a disaster by quickly recovering their applications and data in the cloud.
- Use elastic disaster recovery to minimize downtime and data loss during a disaster.

## Comparing Storage Services

- Amazon S3 - use to store static resources, documents, images, videos
- Amazon EBS - use to scaling block storage for EC2 instances
- Amazon EFS - use for scalable file storage accessible from multiple EC2 instances
