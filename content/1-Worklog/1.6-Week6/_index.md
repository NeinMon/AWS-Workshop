---
title: "Week 6 Worklog"
date: "2024-01-01T00:00:00Z"
weight: 1
chapter: false
pre: " <b> 1.6. </b> "
---

### Week 6 Objectives:

* Learn NoSQL database and Amazon DynamoDB
* Practice with DynamoDB tables, queries, and data modeling
* Explore DynamoDB best practices and advanced features

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --------- | ---------- | --------------- | -------------- |
| 2 | - **Lab05:** Deploy RDS (Part 2 - 4 videos) <br>&emsp; + Create RDS database instance <br>&emsp; + Application Deployment on EC2 <br>&emsp; + Backup and restore testing <br>&emsp; + Clean up resources | 10/14/2025 | 10/14/2025 | [RDS Backup](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_WorkingWithAutomatedBackups.html) |
| 3 | - **Lab39-1:** Hands-on Labs for DynamoDB (intro) <br> - **Lab39-2:** Explore DynamoDB basics <br>&emsp; + NoSQL concepts & use cases <br>&emsp; + Partition key & Sort key design <br>&emsp; + Read/Write capacity units (RCU/WCU) | 10/15/2025 | 10/15/2025 | [DynamoDB Intro](https://docs.aws.amazon.com/dynamodb/) |
| 4 | - **Lab39-3:** Explore DynamoDB Console <br>&emsp; + Create table with keys <br>&emsp; + Add/Update/Delete items <br>&emsp; + Query & Scan operations <br>&emsp; + Create Global Secondary Index (GSI) | 10/16/2025 | 10/16/2025 | [DynamoDB Console](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/ConsoleDynamoDB.html) |
| 5 | - **Lab39-4:** Backup and Restore <br>&emsp; + On-demand backups <br>&emsp; + Point-in-time recovery (PITR) <br>&emsp; + Restore table from backup <br> - **Lab39-5:** Clean up resources | 10/17/2025 | 10/17/2025 | [DynamoDB Backup](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/BackupRestore.html) |
| 6 | - **Lab39-6:** LADV - Advanced Design Patterns <br>&emsp; + One-to-many relationships <br>&emsp; + Many-to-many relationships <br>&emsp; + Composite keys strategies <br>&emsp; + GSI overloading patterns <br>&emsp; + Single-table design | 10/18/2025 | 10/18/2025 | [DynamoDB Patterns](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/bp-general-nosql-design.html) |


### Week 6 Achievements:

**Theoretical Knowledge:**
* ✅ Understand NoSQL database concepts and when to use NoSQL vs SQL
* ✅ Learn Amazon DynamoDB architecture (fully managed NoSQL)
* ✅ Know Partition Key and Sort Key (Primary Key design)
* ✅ Understand Read/Write Capacity Units (RCU/WCU)
* ✅ Study Global Secondary Index (GSI) and Local Secondary Index (LSI)
* ✅ Learn DynamoDB Streams for event-driven architecture
* ✅ Understand DynamoDB design patterns for NoSQL

**Practical Skills:**
* ✅ **DynamoDB Basics:**
  - Create table with partition key + sort key
  - Add/Update/Delete items
  - Query operations (efficient lookup)
  - Scan operations (full table scan)
  - Batch operations (BatchGetItem, BatchWriteItem)
* ✅ **Advanced Features:**
  - Create Global Secondary Index (GSI)
  - Configure on-demand vs provisioned capacity
  - Point-in-time recovery (PITR)
  - On-demand backups and restore
* ✅ **Design Patterns:**
  - One-to-many relationships (partition key design)
  - Many-to-many relationships (GSI overloading)
  - Composite keys (multi-attribute queries)
  - Single-table design pattern
* ✅ **Developer Tools:**
  - AWS CloudShell with DynamoDB CLI
  - Python boto3 SDK for DynamoDB
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
