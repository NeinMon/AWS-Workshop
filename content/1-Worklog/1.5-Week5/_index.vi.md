---
title: "Worklog Tuần 5"
date: "2024-01-01T00:00:00Z"
weight: 1
chapter: false
pre: " <b> 1.5. </b> "
---

### Mục tiêu tuần 5:

* Học về AWS Database services (RDS, Aurora, Redshift, Elasticache)
* Thực hành deploy RDS database instance
* Kết nối application với RDS database

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 2 | - **Lab48:** IAM Role vs Access Key (7 videos) <br>&emsp; + Create EC2 & S3 bucket <br>&emsp; + Generate IAM user & access key <br>&emsp; + Use access key to access S3 <br>&emsp; + Create IAM role for EC2 <br>&emsp; + Using IAM role (best practice) <br>&emsp; + Compare role vs access key | 07/10/2025 | 07/10/2025 | [IAM Roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) |
| 3 | - **Lab44:** Advanced IAM Management (9 videos) <br>&emsp; + Create IAM Groups with policies <br>&emsp; + Create IAM Users & check permissions <br>&emsp; + Create Admin IAM Role with trust policy <br>&emsp; + Configure Switch role <br>&emsp; + Limit switch role by IP address <br>&emsp; + Limit switch role by Time | 08/10/2025 | 08/10/2025 | [IAM Groups](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_groups.html) |
| 4 | - **Lab33:** S3 Encryption với KMS & CloudTrail (Part 1 - 6 videos) <br>&emsp; + Create Policy and Role <br>&emsp; + Create Group and User <br>&emsp; + Create KMS Key for encryption <br>&emsp; + Create S3 Bucket with default encryption <br>&emsp; + Upload encrypted data to S3 <br>&emsp; + Create CloudTrail for logging | 09/10/2025 | 09/10/2025 | [S3 Encryption](https://docs.aws.amazon.com/AmazonS3/latest/userguide/UsingEncryption.html) |
| 5 | - **Lab33:** CloudTrail & Athena (Part 2 - 5 videos) <br>&emsp; + Enable CloudTrail logging <br>&emsp; + Create Amazon Athena database <br>&emsp; + Query CloudTrail logs with Athena SQL <br>&emsp; + Test encrypted data sharing <br>&emsp; + Resource cleanup <br> - **Module 06-01:** Database Concepts review | 10/10/2025 | 10/10/2025 | [CloudTrail](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/) |
| 6 | - **Module 06-02:** Amazon RDS & Amazon Aurora <br> - **Module 06-03:** Redshift & Elasticache <br> - **Lab05:** Deploy RDS (Part 1 - 4 videos) <br>&emsp; + Create VPC for database <br>&emsp; + Create EC2 & RDS Security Groups <br>&emsp; + Create DB Subnet Group <br>&emsp; + Create EC2 instance | 11/10/2025 | 11/10/2025 | [Amazon RDS](https://docs.aws.amazon.com/rds/) |


### Kết quả đạt được tuần 5:

**Kiến thức lý thuyết:**
* ✅ Hiểu các loại database: Relational (SQL) vs NoSQL
* ✅ Nắm về ACID properties và database transactions
* ✅ Biết về Amazon RDS và các database engines (MySQL, PostgreSQL, SQL Server, Oracle)
* ✅ Hiểu Amazon Aurora architecture (MySQL/PostgreSQL compatible)
* ✅ Nắm về Multi-AZ deployment cho high availability
* ✅ Biết về Read Replicas cho scaling read operations
* ✅ Học về Redshift (data warehousing) và Elasticache (caching)

**Kỹ năng thực hành:**
* ✅ **RDS Deployment:**
  - Tạo VPC với subnets phù hợp cho database
  - Cấu hình Security Groups (EC2 & RDS)
  - Tạo DB Subnet Group (Multi-AZ)
  - Launch RDS MySQL instance
  - Cấu hình backup retention và maintenance window
* ✅ **Application Integration:**
  - Deploy web app trên EC2
  - Connect app to RDS database
  - Test database connectivity và queries
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


