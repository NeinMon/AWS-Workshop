---
title: "Worklog Tuần 6"
date: "2024-01-01T00:00:00Z"
weight: 1
chapter: false
pre: " <b> 1.6. </b> "
---

### Mục tiêu tuần 6:

* Học về NoSQL database và Amazon DynamoDB
* Thực hành với DynamoDB tables, queries, và data modeling
* Tìm hiểu DynamoDB best practices và advanced features

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 2 | - **Lab05:** Deploy RDS (Part 2 - 4 videos) <br>&emsp; + Create RDS database instance <br>&emsp; + Application Deployment on EC2 <br>&emsp; + Backup and restore testing <br>&emsp; + Clean up resources | 14/10/2025 | 14/10/2025 | [RDS Backup](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_WorkingWithAutomatedBackups.html) |
| 3 | - **Lab39-1:** Hands-on Labs for DynamoDB (intro) <br> - **Lab39-2:** Explore DynamoDB basics <br>&emsp; + NoSQL concepts & use cases <br>&emsp; + Partition key & Sort key design <br>&emsp; + Read/Write capacity units (RCU/WCU) | 15/10/2025 | 15/10/2025 | [DynamoDB Intro](https://docs.aws.amazon.com/dynamodb/) |
| 4 | - **Lab39-3:** Explore DynamoDB Console <br>&emsp; + Create table with keys <br>&emsp; + Add/Update/Delete items <br>&emsp; + Query & Scan operations <br>&emsp; + Create Global Secondary Index (GSI) | 16/10/2025 | 16/10/2025 | [DynamoDB Console](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/ConsoleDynamoDB.html) |
| 5 | - **Lab39-4:** Backup and Restore <br>&emsp; + On-demand backups <br>&emsp; + Point-in-time recovery (PITR) <br>&emsp; + Restore table from backup <br> - **Lab39-5:** Clean up resources | 17/10/2025 | 17/10/2025 | [DynamoDB Backup](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/BackupRestore.html) |
| 6 | - **Lab39-6:** LADV - Advanced Design Patterns <br>&emsp; + One-to-many relationships <br>&emsp; + Many-to-many relationships <br>&emsp; + Composite keys strategies <br>&emsp; + GSI overloading patterns <br>&emsp; + Single-table design | 18/10/2025 | 18/10/2025 | [DynamoDB Patterns](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/bp-general-nosql-design.html) |


### Kết quả đạt được tuần 6:

**Kiến thức lý thuyết:**
* ✅ Hiểu NoSQL database concepts và khi nào dùng NoSQL vs SQL
* ✅ Nắm về Amazon DynamoDB architecture (fully managed NoSQL)
* ✅ Biết về Partition Key và Sort Key (Primary Key design)
* ✅ Hiểu về Read/Write Capacity Units (RCU/WCU)
* ✅ Học về Global Secondary Index (GSI) và Local Secondary Index (LSI)
* ✅ Nắm DynamoDB Streams cho event-driven architecture
* ✅ Hiểu DynamoDB design patterns cho NoSQL

**Kỹ năng thực hành:**
* ✅ **DynamoDB Basics:**
  - Create table với partition key + sort key
  - Add/Update/Delete items
  - Query operations (efficient lookup)
  - Scan operations (full table scan)
  - Batch operations (BatchGetItem, BatchWriteItem)
* ✅ **Advanced Features:**
  - Tạo Global Secondary Index (GSI)
  - Configure on-demand vs provisioned capacity
  - Point-in-time recovery (PITR)
  - On-demand backups và restore
* ✅ **Design Patterns:**
  - One-to-many relationships (partition key design)
  - Many-to-many relationships (GSI overloading)
  - Composite keys (multi-attribute queries)
  - Single-table design pattern
* ✅ **Developer Tools:**
  - AWS CloudShell với DynamoDB CLI
  - Python boto3 SDK cho DynamoDB
  - Node.js AWS SDK examples

**Tools & Technologies:**
* Amazon DynamoDB Console
* AWS CloudShell
* Python boto3 (DynamoDB SDK)
* Node.js AWS SDK
* DynamoDB Local (testing)
* NoSQL Workbench for DynamoDB
* DynamoDB Streams
* CloudWatch Metrics for DynamoDB


