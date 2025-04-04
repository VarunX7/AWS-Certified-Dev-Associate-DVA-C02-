# EFS : Elastic File System

Serverless file storage service allowing shared access across multiple AWS services and instances.

- Managed NFS(Network File System) that can be mounted onto many EC2 instances in different AZ
- Highly available, scalable, expensive(3x cost), pay per use - no capacity planning
- Uses NFSv4.1 protocol
- Use Security Groups to control access
- Compatible with Linux based AMI only
- Encryption at rest using KMS
- POSIX file system (Linux) that has a standard API
- File system scales automatically
- Use cases - content management, web-serving, data-sharing, Wordpress

![alt text](images/image-2.png)

## Performance Modes

#### Performance Mode (set at EFS creation time)
- **General Purpose(default)** : Latency sensitive use case(web server, CMS, etc.)
- **Max I/O** : higher latency throughput, highly parallel(big data, media processing) 

#### Throughput Mode
- **Bursting** : 1TB = 50MB/s + burst of upto 100MB/s 
- **Provisioned** : Set your throughput regardless of storage size. eg- 1GB/s for 1TB
- **Elastic** : Automatically scales throughput up or down based on your workloads. Used for unpredictable workloads

## Storage Classes

#### Storage Tier
**Standard** : for frequently accessed files
**Infrequent Access(EFS-IA)** : Costs to retrieve files, lower price to store. Used with a lifecycle policy (Files will be moved from std to IA if not accessed for a specified number of days)

#### Availability and durability
**Standard** : Multi-AZ, great for production
**One Zone** : One AZ, great for development, backups are enabled by default, compatible with IA (EFS One-Zone IA) with about 90% discount