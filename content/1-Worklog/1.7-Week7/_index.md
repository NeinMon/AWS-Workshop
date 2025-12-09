---
title: "Week 7 Worklog"
date: "2024-01-01T00:00:00Z"
weight: 1
chapter: false
pre: " <b> 1.7. </b> "
---

### Week 7 Objectives:

* Practice Advanced IAM features (groups, switch roles, tags)
* Learn S3 encryption and KMS integration
* Explore CloudTrail and Athena for security auditing

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --------- | ---------- | --------------- | -------------- |
| 2 | - **Lab39-7:** LMR - Build Global Serverless Application <br>&emsp; + Deploy serverless architecture <br>&emsp; + DynamoDB global tables setup <br>&emsp; + Multi-region replication <br>&emsp; + Lambda functions integration | 10/21/2025 | 10/21/2025 | [DynamoDB Global](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/GlobalTables.html) |
| 3 | - **Lab39-8:** LEDA - Serverless Event Driven Architecture <br>&emsp; + Enable DynamoDB Streams <br>&emsp; + Create Lambda triggers <br>&emsp; + Event-driven patterns <br>&emsp; + Real-time data processing | 10/22/2025 | 10/22/2025 | [DynamoDB Streams](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Streams.html) |
| 4 | - **Lab40:** DynamoDB Data Operations (7 videos) <br>&emsp; + Preparing the database <br>&emsp; + Building a database <br>&emsp; + Data in the Table operations <br>&emsp; + Cost analysis & optimization <br>&emsp; + Tagging and Cost Allocation <br>&emsp; + Usage monitoring <br>&emsp; + Query result analysis | 10/23/2025 | 10/23/2025 | [DynamoDB Operations](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/) |
| 5 | - **Lab60:** DynamoDB with CloudShell, Console & SDK (3 videos) <br>&emsp; + CloudShell commands & CLI <br>&emsp; + Console operations & best practices <br>&emsp; + SDK examples (Python boto3 & Node.js) | 10/24/2025 | 10/24/2025 | [DynamoDB SDK](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/GettingStarted.html) |
| 6 | - **Lab70:** AWS Glue DataBrew (6 videos) <br>&emsp; + Creating a Cloud9 Instance <br>&emsp; + Download & Upload Dataset to S3 <br>&emsp; + Setting up DataBrew <br>&emsp; + Data Profiling <br>&emsp; + Clean & Transform data | 10/25/2025 | 10/25/2025 | [AWS Glue DataBrew](https://docs.aws.amazon.com/databrew/) |


### Week 7 Achievements:

**Theoretical Knowledge:**
* ✅ Learn IAM Groups hierarchy and permission inheritance
* ✅ Understand IAM Role assumption and cross-account access
* ✅ Know Resource Tagging strategy and best practices
* ✅ Study S3 encryption options (SSE-S3, SSE-KMS, SSE-C)
* ✅ Learn AWS KMS (Key Management Service) and CMK
* ✅ Understand AWS CloudTrail for API call logging
* ✅ Know Amazon Athena for querying S3 data

**Practical Skills:**
* ✅ **Advanced IAM:**
  - Create IAM Groups with multiple policies
  - Assign users to groups with role escalation
  - Create Admin IAM Role with trust relationships
  - Configure Switch Role with IP/Time restrictions
  - Test permission boundaries and session policies
* ✅ **Resource Tagging:**
  - Tag EC2 instances (Environment, Owner, Project)
  - Manage tags via Console and CLI
  - Filter resources by tags
  - Create Resource Groups based on tags
  - Tag-based cost allocation
* ✅ **S3 Encryption & KMS:**
  - Create KMS Customer Master Key (CMK)
  - Create S3 bucket with default encryption (SSE-KMS)
  - Upload encrypted objects
  - Share encrypted data with cross-account access
  - Key rotation policy
* ✅ **CloudTrail & Athena:**
  - Enable CloudTrail logging (all API calls)
  - Configure CloudTrail to S3 bucket
  - Create Athena database and table
  - Query CloudTrail logs with SQL (Athena)
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
