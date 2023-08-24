# RDS+Aurora+Elastic Cache

# Amazon RDS overview

- RDS stands for Relational Database Service
- It’s a managed DB service for DB use SQL as a query language
- it allows you to create databases in the cloud that are managed by aws
    - Postgres
    - MYSQL
    - Mariadb
    - Oracle
    - Microsoft SQL Server
    - Aurora

 

# Advantage over using RDS versus deploying DB on EC2

 

- RDS is a managed service
    - Automated provisioning, OS patching
    - Continuous backups and restore to specific timestamp (Point in Time Restore)!
    - Monitoring dashboards
    - Read replicas for improved read performance
    - Multi AZ setup for DR (Disaster Recovery)
    - Maintenance windows for upgrades
    - Scaling capability (vertical and horizontal)
    - Storage backed by EBS (gp2 or io1)
- BUT you can’t SSH into your instances

 

# RDS-Storge Auto Scaling

- Helps you increase storage on your RDS DB instance
dynamically
- When RDS detects you are running out of free database
storage, it scales automatically
- Avoid manually scaling your database storage
- You have to set Maximum Storage Threshold (maximum limit for DB storage)
- Automatically modify storage if
    - Free storage is less than 10% of allocated storage
    - Low-storage lasts at least 5 minutes
    - 6 hours have passed since last modification
- Useful for applications with unpredictable workloads
- Supports all RDS database engines (MariaDB, MySQL,
PostgreSQL, SQL Server, Oracle)

# RDS Read Replicas for read scalability

- Up to 15 Read Replicas
- Within AZ, Cross AZ or
Cross Region
- Replication is ASYNC,
so reads are eventually
consistent
- Replicas can be
promoted to their own
DB
- Applications must
update the connection
string to leverage read
replicas
![Screenshot 2023-08-24 083912](https://github.com/alinedam/Sitech-Internship/assets/108859223/bad004b2-04a7-43aa-9fb8-42eb99476fd2)



# RDS Read Replicas – Use Cases

- You have a production database
that is taking on normal load
- You want to run a reporting
application to run some analytics
- You create a Read Replica to run
the new workload there
- The production application is
unaffected
- Read replicas are used for SELECT
(=read) only kind of statements
(not INSERT, UPDATE, DELETE)

![Screenshot 2023-08-24 084042](https://github.com/alinedam/Sitech-Internship/assets/108859223/2bd99f92-d246-4115-b174-a36d9d9a1021)


# RDS Read Replicas – Network Cost

- In AWS there’s a network cost when data goes from one AZ to another
- For RDS Read Replicas within the same region, you don’t pay that fee

![Screenshot 2023-08-24 084139](https://github.com/alinedam/Sitech-Internship/assets/108859223/10f5e1d1-e226-4019-8f43-c30a3445bea7)


# RDS Multi AZ (Disaster Recovery)

- SYNC replication
- One DNS name – automatic app
failover to standby
- Failover in case of loss of AZ, loss of
network, instance or storage failure
- No manual intervention in apps
- Not used for scaling
- Note:The Read Replicas be setup as
Multi AZ for Disaster Recovery (DR)

![Screenshot 2023-08-24 084303](https://github.com/alinedam/Sitech-Internship/assets/108859223/e55e14b5-ffe2-4b27-be34-b4dedfff277a)



# RDS – From Single-AZ to Multi-AZ

- Zero downtime operation (no
need to stop the DB)
- Just click on “modify” for the
database
- The following happens internally
    - A snapshot is taken
    - A new DB is restored from the
    snapshot in a new AZ
    - Synchronization is established
    between the two databases

# RDS Custom

- Managed Oracle and Microsoft SQL Server Database with OS
and database customization
- RDS: Automates setup, operation, and scaling of database in AWS
- Custom: access to the underlying database and OS so you can
    - Configure settings
    - Install patches
    - Enable native features
    - Access the underlying EC2 Instance using SSH or SSM Session Manager
- De-activate Automation Mode to perform your customization, better to take a DB snapshot before
- RDS vs. RDS Custom
- RDS: entire database and the OS to be managed by AWS
- RDS Custom: full admin access to the underlying OS and the database

# Amazon Aurora

- Automatic fail-over
- Backup and Recovery
- Isolation and security
- Industry compliance
- Push-button scalinRDS, Aurora & ElastiCacheg
- Automated Patching with Zero Downtime
- Advanced Monitoring
- Routine Maintenance
- Backtrack: restore data at any point of time without using backups
