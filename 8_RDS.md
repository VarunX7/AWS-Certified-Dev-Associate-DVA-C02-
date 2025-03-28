# RDS: Relational Database Service

A managed DB service for DB use SQL a query

It allows you to create databases in the cloud that are
* Postgres
* Oracle
* MySQL
* MariaDB
* Microsoft SQL Server
* Aurora (AWS proprietary database)

#### Advantages of RDS over deploying a database in EC2
* Managed service
    * Automated provisioning, OS patching level
    * Continuous backups and restore to specific timestamps (Point in Time Restore)
    * Monitoring dashboards
    * Read replicas for improved read performance
    * Multi AZ setup for DR (Disaster Recovery)
    * Maintenance windows for upgrades
    * Scaling capability (vertical and horizontal)
    * Storage backed by EBS (gp2 or IO1)
* But you can’t SSH into your instances (amazon manages them for you)

#### Storage Auto scaling
* Increases the storage in your RDS DB automatically if it detects that free storage is running out
* You have to set a Maximum Storage Threshold (max limit for db storage)
* Automatically modify storage if
    * Free storage is less than 10%
    * Low-storage lasts at least 5 min
    * 6 hours have passed since last modification
* Useful for applications with unpredictable workloads

#### RDS Read replicas for read scalability
* Up to 15 read replicas(MySQL, MariaDB and PostgreSQL) and 5 read replicas(Oracle and SQL Server)
* Within AZ, Cross AZ or Cross region
* Replication is Async, so reads are eventually consistent
* Replicas can be promoted to their own DB
* Applications must update the connection string to leverage read replicas
* To enable read replicas, you need to enable backups

RDS Read Replicas – Use Cases
- You have a production database that is taking on normal load
- You want to run a reporting application to run some analytics
- You create a Read Replica to run the new workload there
- The production application is unaffected
- Read replicas are used for SELECT (=read) only kind of statements (not INSERT, UPDATE, DELETE)

RDS Read Replicas – Network Cost
- In AWS there’s a network cost when data goes from one AZ to another 
- You don't pay the fee for data transfer between Read Replicas in the same region(even if they are in different AZ)
- You pay a fee if the replicas are in different regions

#### RDS Multi AZ (Disaster Recovery)
* SYNC replication
* One DNS name - automatic app failover to standby
* Increase availability
* Failover in case of loss of AZ, loss of network, instance or storage failure
* No manual intervention in apps
* Not used for scaling (only disaster recovery)
* Can easily "modify" from single-AZ to Multi-AZ with zero downtime(no need to stop the DB)

#### RDS Backups
* Backups are automatically enabled in RDS
* Automated backups:
    * Daily full snapshot of the database
    * Capture transaction logs in real time
    * Ability to restore to any point in time
    * 7 days retention (can be increased to 35 days)
* DB Snapshots:
    * Manually triggered by the user
    * Retention of backup for as long as you want

#### RDS Encryption
* Encryption at rest capability with AWS KMS 
* SSL/TLS certificates to encrypt data to RDS in flight
* To enforce SSL:
    * PostgreSQL: rds.force_ssl=1 in the AWS RDS Console (parameter groups)
* TO connect using SSL:
    * Provide the SSL Trust certificate (can be downloaded from AWS)
    * Provide SSL options when connection to the database
	
RDS Encryption Operations
- Encrypting RDS backups
	- Snapshots of un-encrypted RDS databases are un-encrypted 
	- Snapshots of encrypted RDS databases are encrypted
	- Can copy a snapshot into an encrypted one
- To encrypt an un-encrypted RDS database: 
	- Create a snapshot of the un-encrypted database
	- Copy the snapshot and enable encryption for the snapshot
	- Restore the database from the encrypted snapshot
	- Migrate applications to the new database, and delete the old database

#### RDS Proxy
Many applications open and close DB connections frequently, leading to high load on the DB's resources
RDS Proxy reuses and pools connections so your DB isn't overwhelmed by excessive connection handling

- Fully managed database proxy
- Allows apps to pool and share connections with DB
- Improves DB's efficiency by reducing stress on the DB's resources(eg- CPU, RAM) and minimizing the open connections (and timeouts)
- Serverless, autoscaling and highly available(Multi-AZ)
- Reduces RDS and Aurora failover time by 66%
- Supports all RDS DBs and Aurora
- No code changes required for most apps
- Enforce IAM auth for DB and securely stores credentials in AWS Secrets Manager
- RDS Proxy is never publically accessible (must be accessed from VPC)

#### RDS Security
* RDS databases are usually deployed within a private subnet, not in a public one
* RDS Security works by leveraging security groups (the same concept as for EC2 instances) - it controls who can communicate with RDS
* IAM policies help control who can manage RDS
* Traditional username and password can be used to login to the database
* IAM users can now be used too (for MySQL / Aurora - New)

