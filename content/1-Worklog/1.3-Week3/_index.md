---
title: "Week 3 Worklog"
date: "2024-01-01T00:00:00Z"
weight: 1
chapter: false
pre: " <b> 1.3. </b> "
---

### Week 3 Objectives:

* Deep dive into AWS Storage services and architecture
* Practice with Amazon S3 and advanced features
* Learn about Snow Family, Storage Gateway, and AWS Backup
* Practice migration and hybrid storage solutions

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                   | Start Date | Completion Date | Reference Material                            |
| --- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ----------------------------------------- |
| 2   | - **Theory:** Module 04-01 Storage Services on AWS <br> - **AWS Backup Practice:** <br>&emsp; + Create S3 Bucket <br>&emsp; + Deploy Infrastructure <br>&emsp; + Create Backup Plan <br>&emsp; + Set up notifications | 23/09/2025   | 23/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 3   | - **Theory:** Module 04-02 Amazon S3 - Access Point - Storage Class <br> - **VM Migration Practice:** <br>&emsp; + VMWare Workstation setup <br>&emsp; + Export VM from On-premises <br>&emsp; + Upload and Import VM to AWS <br>&emsp; + Deploy Instance from AMI | 24/09/2025   | 24/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - **Theory:** Module 04-03 S3 Static Website & CORS - Control Access <br> - **Storage Gateway Practice:** <br>&emsp; + Create Storage Gateway <br>&emsp; + Create File Shares <br>&emsp; + Mount File shares on On-premises <br> - **Test Backup Restore** | 25/09/2025   | 25/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - **Theory:** Module 04-04 Snow Family - Storage Gateway - Backup <br> - **FSx Practice:** <br>&emsp; + Create SSD and HDD Multi-AZ file system <br>&emsp; + Test Performance and Monitor <br>&emsp; + Enable data deduplication <br>&emsp; + Manage user sessions and quotas | 26/09/2025   | 26/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - **S3 Advanced Practice:** <br>&emsp; + Create S3 static website <br>&emsp; + Configure CloudFront <br>&emsp; + Bucket Versioning <br>&emsp; + Cross-Region Replication <br> - **Scale FSx capacity** <br> - **Clean up all resources** | 27/09/2025   | 27/09/2025      | <https://cloudjourney.awsstudygroup.com/> |


### Week 3 Achievements:

#### **📚 Theoretical Knowledge Acquired:**

* **AWS Storage Services Overview:**
  * Understand AWS storage services ecosystem (S3, EBS, EFS, FSx)
  * Know storage classes and use cases
  * Learn how to choose appropriate storage solution for each scenario
  * Understand storage performance and cost optimization

* **Amazon Simple Storage Service (S3):**
  * Master S3 architecture and concepts (buckets, objects, keys)
  * Understand S3 storage classes (Standard, IA, Glacier, Deep Archive)
  * Learn S3 Access Points and multi-region access patterns
  * Know S3 security features and access control methods

* **S3 Advanced Features:**
  * Static website hosting and CORS configuration
  * Object versioning and lifecycle management
  * Cross-Region Replication (CRR) setup and monitoring
  * CloudFront integration for content delivery

* **Hybrid and Migration Solutions:**
  * Snow Family (Snowball, Snowmobile) for data migration
  * AWS Storage Gateway (File, Volume, Tape Gateway)
  * VM Import/Export processes
  * Hybrid cloud storage architectures

#### **🛠️ Practical Skills Achieved:**

* **AWS Backup and Data Protection:**
  * Created and configured S3 buckets with proper naming conventions
  * Set up backup plans with schedules and retention policies
  * Configured backup notifications and monitoring
  * Performed restore testing to verify backup integrity

* **Virtual Machine Migration:**
  * Set up VMware Workstation environment
  * Exported VMs from on-premises infrastructure
  * Uploaded large VM files to AWS S3
  * Imported VMs and created AMIs from uploaded files
  * Deployed EC2 instances from custom AMIs
  * Configured S3 bucket ACLs for VM export/import

* **AWS Storage Gateway:**
  * Deployed Storage Gateway in hybrid environment
  * Created and configured file shares
  * Mounted NFS/SMB shares on on-premises machines
  * Tested file synchronization between on-prem and cloud
  * Monitored storage gateway performance and usage

* **Amazon FSx Implementation:**
  * Created FSx file systems (SSD and HDD Multi-AZ)
  * Configured file shares with proper permissions
  * Tested performance and monitoring
  * Enabled advanced features:
    * Data deduplication for space optimization
    * Shadow copies for versioning
    * User storage quotas management
    * User sessions and open files monitoring
  * Scaled throughput and storage capacity dynamically

* **S3 Static Website and CDN:**
  * Set up S3 static website hosting
  * Configured public access blocks and bucket policies
  * Integrated Amazon CloudFront for global content delivery
  * Tested website performance and availability
  * Implemented security best practices (block public access + CloudFront)

* **S3 Advanced Data Management:**
  * Enabled S3 bucket versioning
  * Configured lifecycle policies for object transitions
  * Set up Cross-Region Replication for disaster recovery
  * Tested object movement between storage classes
  * Monitored replication status and metrics

#### **🔧 Tools and Technologies Used:**
* **AWS Services:** S3, CloudFront, Storage Gateway, FSx, AWS Backup
* **Migration Tools:** VM Import/Export, Snow Family concepts
* **Monitoring:** CloudWatch, S3 metrics, FSx performance monitoring
* **Virtualization:** VMware Workstation, AMI creation
* **Hybrid Connectivity:** File Gateway, NFS/SMB protocols

#### **💡 Insights and Best Practices Learned:**

* **Storage Strategy:**
  * Right-sizing storage for cost optimization
  * Lifecycle policies for automatic tiering
  * Multi-region strategy for disaster recovery
  * Performance vs cost trade-offs

* **Data Migration:**
  * Planning for large-scale migrations
  * Network bandwidth considerations
  * Downtime minimization strategies
  * Testing and validation processes

* **Security and Compliance:**
  * S3 bucket security best practices
  * Access control with IAM, bucket policies, ACLs
  * Encryption in transit and at rest
  * Monitoring and auditing access patterns

* **Performance Optimization:**
  * S3 performance optimization techniques
  * FSx performance tuning
  * CDN caching strategies
  * Storage gateway optimization

#### **🎯 Completion Level:**
* **Theory:** 100% - Completed modules 04-01 to 04-04
* **Practice:** 100% - Completed 39 labs from backup to replication
* **Overall:** Mastery level in AWS Storage services

#### **🚀 Preparation for Next Week:**
* Learn AWS Security & IAM (Identity and Access Management)
* Study IAM users, groups, roles, and policies
* Practice with AWS Organizations and Identity Center
* Explore AWS Security Hub and compliance best practices
* Prepare for hands-on labs on resource tagging and IAM role switching
