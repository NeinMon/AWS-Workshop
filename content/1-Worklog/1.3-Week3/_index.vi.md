---
title: "Worklog Tuần 3"
date: "2024-01-01T00:00:00Z"
weight: 1
chapter: false
pre: " <b> 1.3. </b> "
---

### Mục tiêu tuần 3:

* Tìm hiểu sâu về AWS Storage services và architecture
* Thực hành với Amazon S3 và các tính năng advanced
* Học về Snow Family, Storage Gateway và AWS Backup
* Thực hành migration và hybrid storage solutions

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - **Học lý thuyết:** Module 04-01 Dịch Vụ Lưu Trữ Trên AWS <br> - **Thực hành AWS Backup:** <br>&emsp; + Create S3 Bucket <br>&emsp; + Deploy Infrastructure <br>&emsp; + Create Backup Plan <br>&emsp; + Set up notifications | 23/09/2025   | 23/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 3   | - **Học lý thuyết:** Module 04-02 Amazon S3 - Access Point - Storage Class <br> - **Thực hành VM Migration:** <br>&emsp; + VMWare Workstation setup <br>&emsp; + Export VM from On-premises <br>&emsp; + Upload và Import VM to AWS <br>&emsp; + Deploy Instance from AMI | 24/09/2025   | 24/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - **Học lý thuyết:** Module 04-03 S3 Static Website & CORS - Control Access <br> - **Thực hành Storage Gateway:** <br>&emsp; + Create Storage Gateway <br>&emsp; + Create File Shares <br>&emsp; + Mount File shares on On-premises <br> - **Test Backup Restore** | 25/09/2025   | 25/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - **Học lý thuyết:** Module 04-04 Snow Family - Storage Gateway - Backup <br> - **Thực hành FSx:** <br>&emsp; + Create SSD và HDD Multi-AZ file system <br>&emsp; + Test Performance và Monitor <br>&emsp; + Enable data deduplication <br>&emsp; + Manage user sessions và quotas | 26/09/2025   | 26/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - **Thực hành S3 Advanced:** <br>&emsp; + Create S3 static website <br>&emsp; + Configure CloudFront <br>&emsp; + Bucket Versioning <br>&emsp; + Cross-Region Replication <br> - **Scale FSx capacity** <br> - **Clean up all resources** | 27/09/2025   | 27/09/2025      | <https://cloudjourney.awsstudygroup.com/> |


### Kết quả đạt được tuần 3:

#### **📚 Kiến thức lý thuyết đã nắm được:**

* **AWS Storage Services Overview:**
  * Hiểu ecosystem storage services của AWS (S3, EBS, EFS, FSx)
  * Nắm được các storage classes và use cases
  * Biết cách chọn storage solution phù hợp cho từng scenario
  * Hiểu về storage performance và cost optimization

* **Amazon Simple Storage Service (S3):**
  * Nắm vững S3 architecture và concepts (buckets, objects, keys)
  * Hiểu các S3 storage classes (Standard, IA, Glacier, Deep Archive)
  * Tìm hiểu S3 Access Points và multi-region access patterns
  * Biết về S3 security features và access control methods

* **S3 Advanced Features:**
  * Static website hosting và CORS configuration
  * Object versioning và lifecycle management
  * Cross-Region Replication (CRR) setup và monitoring
  * CloudFront integration cho content delivery

* **Hybrid và Migration Solutions:**
  * Snow Family (Snowball, Snowmobile) cho data migration
  * AWS Storage Gateway (File, Volume, Tape Gateway)
  * VM Import/Export processes
  * Hybrid cloud storage architectures

#### **🛠️ Kỹ năng thực hành đã đạt được:**

* **AWS Backup và Data Protection:**
  * Tạo và cấu hình S3 buckets với proper naming conventions
  * Thiết lập backup plans với schedules và retention policies
  * Cấu hình backup notifications và monitoring
  * Thực hiện restore testing để verify backup integrity

* **Virtual Machine Migration:**
  * Setup VMware Workstation environment
  * Export VMs từ on-premises infrastructure
  * Upload large VM files lên AWS S3
  * Import VMs và tạo AMIs từ uploaded files
  * Deploy EC2 instances từ custom AMIs
  * Cấu hình S3 bucket ACLs cho VM export/import

* **AWS Storage Gateway:**
  * Deploy Storage Gateway trong hybrid environment
  * Tạo và cấu hình file shares
  * Mount NFS/SMB shares trên on-premises machines
  * Test file synchronization giữa on-prem và cloud
  * Monitor storage gateway performance và usage

* **Amazon FSx Implementation:**
  * Tạo FSx file systems (SSD và HDD Multi-AZ)
  * Cấu hình file shares với proper permisstestingions
  * Performance  và monitoring
  * Enable advanced features:
    * Data deduplication cho space optimization
    * Shadow copies cho versioning
    * User storage quotas management
    * User sessions và open files monitoring
  * Scale throughput và storage capacity dynamically

* **S3 Static Website và CDN:**
  * Setup S3 static website hosting
  * Cấu hình public access blocks và bucket policies
  * Integrate Amazon CloudFront cho global content delivery
  * Test website performance và availability
  * Implement security best practices (block public access + CloudFront)

* **S3 Advanced Data Management:**
  * Enable S3 bucket versioning
  * Configure lifecycle policies cho object transitions
  * Setup Cross-Region Replication cho disaster recovery
  * Test object movement giữa storage classes
  * Monitor replication status và metrics

#### **🔧 Tools và Technologies đã sử dụng:**
* **AWS Services:** S3, CloudFront, Storage Gateway, FSx, AWS Backup
* **Migration Tools:** VM Import/Export, Snow Family concepts
* **Monitoring:** CloudWatch, S3 metrics, FSx performance monitoring
* **Virtualization:** VMware Workstation, AMI creation
* **Hybrid Connectivity:** File Gateway, NFS/SMB protocols

#### **💡 Insights và Best Practices học được:**

* **Storage Strategy:**
  * Right-sizing storage cho cost optimization
  * Lifecycle policies để automatic tiering
  * Multi-region strategy cho disaster recovery
  * Performance vs cost trade-offs

* **Data Migration:**
  * Planning cho large-scale migrations
  * Network bandwidth considerations
  * Downtime minimization strategies
  * Testing và validation processes

* **Security và Compliance:**
  * S3 bucket security best practices
  * Access control với IAM, bucket policies, ACLs
  * Encryption in transit và at rest
  * Monitoring và auditing access patterns

* **Performance Optimization:**
  * S3 performance optimization techniques
  * FSx performance tuning
  * CDN caching strategies
  * Storage gateway optimization

#### **🎯 Mức độ hoàn thành:**
* **Lý thuyết:** 100% - Hoàn thành modules 04-01 đến 04-04
* **Thực hành:** 100% - Hoàn thành 39 labs từ backup đến replication
* **Overall:** Mastery level trong AWS Storage services

#### **🚀 Chuẩn bị cho tuần tiếp theo:**
* Tìm hiểu AWS Security & IAM (Identity and Access Management)
* Học về IAM users, groups, roles, và policies
* Thực hành với AWS Organizations và Identity Center
* Khám phá AWS Security Hub và compliance best practices
* Chuẩn bị cho hands-on labs về resource tagging và IAM role switching


