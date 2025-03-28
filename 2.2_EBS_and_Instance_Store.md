# EBS Volume

* An EC2 machine loses its root volume (main drive) when it is manually terminated.
* Unexpected terminations might happen from time to time (AWS would email you)
* Sometimes, you need a way to store your instance data somewhere
* An EBS (Elastic Block Store) Volume is a network drive you can attach to your instances while they run
* It allows your instances to persist data

#### EBS Volume
* It’s a network drive (Not a physical drive)
    * It uses the network to communicate the instance, which means there might be a bit of latency
    * It can be detached from an EC2 instance and attached to another one quickly
* It’s locked to an Availability Zone (AZ)
    * An EBS Volume in us-east-1a cannot be attached to us-east-1b
    * To move a volume across, you first need to snapshot it then copy it to other regions or AZ
* Have a provisioned capacity (size in GBs and IOPs)
    * You get billed for all the provisioned capacity
    * You can increase the capacity of the drive over time
* Delete on Termination Attribute
    * By default, the root volume is deleted when EC2 instance terminates(attribute enabled)
    * By default, any other attached EC2 volume is not deleted(attribute disabled)

#### EBS Volume Types
- EBS Volumes come in 4 types 
- GP2/GP3 (SSD): General purpose SSD volume that balances price and performance for a wide variety of workloads 
- IO1/IO2 (SSD): Highest-performance SSD volume for mission-critical low-latency or high-throughput workloads 
- ST1 (HDD): Low cost HDD volume designed for frequently accessed, throughput-intensive workloads 
- SC1 (HDD): Lowest cost HDD volume designed for less frequently accessed workloads 
- EBS Volumes are characterized in Size | Throughput | IOPS
- When in doubt always consult the AWS documentation
-  Only GP2/GP3 and IO1/IO2 can be used as boot volumes

#### EBS Volume Types Use Cases
1. GP2/GP3
- Recommended for most workloads 
- System boot volumes
- Virtual desktops
- Low-latency interactive apps
- Development and test environments
- Max 16000 IOPS

2. IO1/IO2 (Provisioned IOPS)
- 4GB - 16TB
- Critical business applications that require sustained IOPS performance, or more than 16,000 IOPS per volume (gp2/gp3 limit)
- Large database workloads, such as: MongoDB, Cassandra, Microsoft SQL Server, MySQL, PostgreSQL, Oracle
- Max PIOPS 64,000 for Nitro EC2 instances and 32,000 for others.
- Can increase PIOPS independently from storage size.
- IO2 Block Express(4GB - 64TB): Sub-milisecond latency. 256,000 max IOPS with 1000:1 IOPS:GB ratio.

3. ST1 
- Streaming workloads requiring consistent, fast throughput at a low price. 
- Big data, Data warehouses, Log processing
- Apache Kafka
- Cannot be a boot volume
- Max throughput 500MB/s - max IOPS 500
 
4. SC1
- Throughput-oriented storage for large volumes of data that is infrequently accessed
- Scenarios where the lowest storage cost is important
- Cannot be a boot volume
- Max throughput 250MB/s - max IOPS 250

#### EBS Volume Types Summary
- gp2/gp3: General Purpose Volumes (cheap)
- io1/io2: Provisioned IOPS (expensive)
- st1: Throughput Optimized HDD
- sc1: Cold HDD, Infrequently accessed data

#### EBS Multi-Attach
- io1/ io2 only.
- Can attach one EBS volume to upto 16 EC2 instances in the same AZ.
- Use Cases 
    - Achieve higher application availability in clustured Linux applications(eg- Teradata)
    - Applications that must maintain concurrent write operations
- Must use a file system that's clustur-aware(not XFS, EXT4, etc.)

#### EBS Snapshots
* EBS Volumes can be backed up using “snapshots”
* Snapshots only take the actual space of the blocks on the volume
* If you take a snapshot of a 100GB drive that only has 5 gb of data, then your EBS snapshot will only be 5 gb
* Snapshots are used for:
    * Backups: ensuring you can save your data in case of catastrophe
    * Volume migration
        * Resizing a volume down
        * Changing the volume type
        * Encrypt a volume

* Archive tier snapshots are 75% cheaper but take 24 to 72 hours for restoring the archive
* Setup a recycle bin to recover snapshots after accidental deletion (1day to 1yr retention)
* Fast Snapshot Restore (FSR): Force full initialisation of the snapshot to have no latency on the first use. Very expensive.

#### EBS Encryption
* When you create an encrypted EBS volume, you get the following:
    * Data at rest is encrypted inside the volume
    * All the data in flight moving between the instance and the volume is encrypted
    * All snapshots are encrypted
    * All volumes created from the snapshots are encrypted
* Encryption and decryption are handled transparently (you have nothing to do)
* Encryption has a minimal impact on latency
* EBS Encryption leverages keys from KMS (AES-256)
* Copying an unencrypted snapshot allows encryption

#### Instance Store
* Physically attached to the machine thus providing a very high performance hardware disk
* Some instance do not come with Root EBS volumes. Instead, they come with “instance Store”
* Pros:
    * Better I/O performance than EBS
* Cons:
    * On termination, the instance store is lost(ephemeral)
    * You can’t resize the instance store
    * Risk of data loss if hardware fails
    * Backups must be operated by the user
* Good for buffer/ cache/ scratch data/ temporary content but not for long term storage
* Overall, EBS-backed instances should fit most applications workloads

#### EBS Summary

* EBS are locked at the AZ level
* Migrating an EBS volume across AZ means first backing it up (snapshot), then recreating it in the other AZ
* EBS backups use IO and you shouldn’t run them while your application is handling a lot of traffic
* Root EBS Volumes of instances get terminated by default if the EC2 instance gets terminated. (You can disable that)
