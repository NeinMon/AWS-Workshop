---
title: "Worklog Tuần 7"
date: "2024-01-01T00:00:00Z"
weight: 1
chapter: false
pre: " <b> 1.7. </b> "
---

### Mục tiêu tuần 7:

* Thực hành Advanced IAM features (groups, switch roles, tags)
* Học về S3 encryption và KMS integration
* Tìm hiểu CloudTrail và Athena cho security auditing

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 2 | - **Lab39-7:** LMR - Build Global Serverless Application <br>&emsp; + Deploy serverless architecture <br>&emsp; + DynamoDB global tables setup <br>&emsp; + Multi-region replication <br>&emsp; + Lambda functions integration | 21/10/2025 | 21/10/2025 | [DynamoDB Global](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/GlobalTables.html) |
| 3 | - **Lab39-8:** LEDA - Serverless Event Driven Architecture <br>&emsp; + Enable DynamoDB Streams <br>&emsp; + Create Lambda triggers <br>&emsp; + Event-driven patterns <br>&emsp; + Real-time data processing | 22/10/2025 | 22/10/2025 | [DynamoDB Streams](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Streams.html) |
| 4 | - **Lab40:** DynamoDB Data Operations (7 videos) <br>&emsp; + Preparing the database <br>&emsp; + Building a database <br>&emsp; + Data in the Table operations <br>&emsp; + Cost analysis & optimization <br>&emsp; + Tagging and Cost Allocation <br>&emsp; + Usage monitoring <br>&emsp; + Query result analysis | 23/10/2025 | 23/10/2025 | [DynamoDB Operations](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/) |
| 5 | - **Lab60:** DynamoDB với CloudShell, Console & SDK (3 videos) <br>&emsp; + CloudShell commands & CLI <br>&emsp; + Console operations & best practices <br>&emsp; + SDK examples (Python boto3 & Node.js) | 24/10/2025 | 24/10/2025 | [DynamoDB SDK](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/GettingStarted.html) |
| 6 | - **Lab70:** AWS Glue DataBrew (6 videos) <br>&emsp; + Creating a Cloud9 Instance <br>&emsp; + Download & Upload Dataset to S3 <br>&emsp; + Setting up DataBrew <br>&emsp; + Data Profiling <br>&emsp; + Clean & Transform data | 25/10/2025 | 25/10/2025 | [AWS Glue DataBrew](https://docs.aws.amazon.com/databrew/) |


### Kết quả đạt được tuần 7:

**Kiến thức lý thuyết:**
* ✅ Nắm về IAM Groups hierarchy và permission inheritance
* ✅ Hiểu IAM Role assumption và cross-account access
* ✅ Biết về Resource Tagging strategy và best practices
* ✅ Học về S3 encryption options (SSE-S3, SSE-KMS, SSE-C)
* ✅ Nắm về AWS KMS (Key Management Service) và CMK
* ✅ Hiểu AWS CloudTrail cho API call logging
* ✅ Biết về Amazon Athena cho querying S3 data

**Kỹ năng thực hành:**
* ✅ **Advanced IAM:**
  - Create IAM Groups với multiple policies
  - Assign users to groups với role escalation
  - Create Admin IAM Role với trust relationships
  - Configure Switch Role với IP/Time restrictions
  - Test permission boundaries và session policies
* ✅ **Resource Tagging:**
  - Tag EC2 instances (Environment, Owner, Project)
  - Manage tags via Console và CLI
  - Filter resources by tags
  - Create Resource Groups based on tags
  - Tag-based cost allocation
* ✅ **S3 Encryption & KMS:**
  - Create KMS Customer Master Key (CMK)
  - Create S3 bucket với default encryption (SSE-KMS)
  - Upload encrypted objects
  - Share encrypted data với cross-account access
  - Key rotation policy
* ✅ **CloudTrail & Athena:**
  - Enable CloudTrail logging (all API calls)
  - Configure CloudTrail to S3 bucket
  - Create Athena database và table
  - Query CloudTrail logs với SQL (Athena)
  - Analyze security events (who did what, when)

**Tools & Technologies:**
* IAM Console (Groups, Roles, Policies)
* AWS Resource Groups & Tag Editor
* AWS KMS Console
* Amazon S3 with encryption
* AWS CloudTrail
* Amazon Athena (SQL queries)
* AWS CLI for automation
* AWS Glue DataBrew for data transformation
* Cloud9 IDE for development


