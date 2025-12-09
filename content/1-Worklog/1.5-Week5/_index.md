---
title: "Week 5 Worklog"
date: "2024-01-01T00:00:00Z"
weight: 1
chapter: false
pre: " <b> 1.5. </b> "
---

### Week 5 Objectives:

* Learn AWS Database services (RDS, Aurora, Redshift, Elasticache)
* Practice deploying RDS database instance
* Connect application to RDS database

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --------- | ---------- | --------------- | -------------- |
| 2 | - **Lab48:** IAM Role vs Access Key (7 videos) <br>&emsp; + Create EC2 & S3 bucket <br>&emsp; + Generate IAM user & access key <br>&emsp; + Use access key to access S3 <br>&emsp; + Create IAM role for EC2 <br>&emsp; + Using IAM role (best practice) <br>&emsp; + Compare role vs access key | 10/07/2025 | 10/07/2025 | [IAM Roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) |
| 3 | - **Lab44:** Advanced IAM Management (9 videos) <br>&emsp; + Create IAM Groups with policies <br>&emsp; + Create IAM Users & check permissions <br>&emsp; + Create Admin IAM Role with trust policy <br>&emsp; + Configure Switch role <br>&emsp; + Limit switch role by IP address <br>&emsp; + Limit switch role by Time | 10/08/2025 | 10/08/2025 | [IAM Groups](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_groups.html) |
| 4 | - **Lab33:** S3 Encryption with KMS & CloudTrail (Part 1 - 6 videos) <br>&emsp; + Create Policy and Role <br>&emsp; + Create Group and User <br>&emsp; + Create KMS Key for encryption <br>&emsp; + Create S3 Bucket with default encryption <br>&emsp; + Upload encrypted data to S3 <br>&emsp; + Create CloudTrail for logging | 10/09/2025 | 10/09/2025 | [S3 Encryption](https://docs.aws.amazon.com/AmazonS3/latest/userguide/UsingEncryption.html) |
| 5 | - **Lab33:** CloudTrail & Athena (Part 2 - 5 videos) <br>&emsp; + Enable CloudTrail logging <br>&emsp; + Create Amazon Athena database <br>&emsp; + Query CloudTrail logs with Athena SQL <br>&emsp; + Test encrypted data sharing <br>&emsp; + Resource cleanup <br> - **Module 06-01:** Database Concepts review | 10/10/2025 | 10/10/2025 | [CloudTrail](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/) |
| 6 | - **Module 06-02:** Amazon RDS & Amazon Aurora <br> - **Module 06-03:** Redshift & Elasticache <br> - **Lab05:** Deploy RDS (Part 1 - 4 videos) <br>&emsp; + Create VPC for database <br>&emsp; + Create EC2 & RDS Security Groups <br>&emsp; + Create DB Subnet Group <br>&emsp; + Create EC2 instance | 10/11/2025 | 10/11/2025 | [Amazon RDS](https://docs.aws.amazon.com/rds/) |


### Week 5 Achievements:

**Theoretical Knowledge:**
* ✅ Understand database types: Relational (SQL) vs NoSQL
* ✅ Learn ACID properties and database transactions
* ✅ Know Amazon RDS and database engines (MySQL, PostgreSQL, SQL Server, Oracle)
* ✅ Understand Amazon Aurora architecture (MySQL/PostgreSQL compatible)
* ✅ Learn Multi-AZ deployment for high availability
* ✅ Know Read Replicas for scaling read operations
* ✅ Study Redshift (data warehousing) and Elasticache (caching)

**Practical Skills:**
* ✅ **RDS Deployment:**
  - Create VPC with appropriate subnets for database
  - Configure Security Groups (EC2 & RDS)
  - Create DB Subnet Group (Multi-AZ)
  - Launch RDS MySQL instance
  - Configure backup retention and maintenance window
* ✅ **Application Integration:**
  - Deploy web app on EC2
  - Connect app to RDS database
  - Test database connectivity and queries
* ✅ **Backup & Restore:**
  - Manual snapshot
  - Automated backups
  - Point-in-time recovery
  - Restore database from snapshot

**Tools & Technologies:**
* Amazon RDS Console
* MySQL Workbench / pgAdmin
* Database connection from EC2
* RDS Parameter Groups
* CloudWatch for database monitoring
* AWS Backup service
* RDS automated snapshots
* AWS CLI for RDS management
