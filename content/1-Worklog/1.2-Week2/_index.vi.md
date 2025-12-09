---
title: "Worklog Tuần 2"
date: "2024-01-01T00:00:00Z"
weight: 1
chapter: false
pre: " <b> 1.2. </b> "
---


### Mục tiêu tuần 2:

* Tìm hiểu sâu về AWS VPC và networking services
* Thực hành tạo và cấu hình VPC infrastructure
* Học về VPC Security, VPN, DirectConnect và Load Balancer
* Thực hành các kịch bản networking phức tạp

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - **Học lý thuyết:** Module 02-01 AWS Virtual Private Cloud <br> - **Thực hành VPC cơ bản:** <br>&emsp; + Create VPC <br>&emsp; + Create Subnet <br>&emsp; + Create Internet Gateway <br>&emsp; + Create Route Table <br>&emsp; + Create Security Groups | 16/09/2025   | 16/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 3   | - **Học lý thuyết:** Module 02-02 VPC Security and Multi-VPC features <br> - **Thực hành EC2 trong VPC:** <br>&emsp; + Create EC2 Instances in Subnets <br>&emsp; + Test connection <br>&emsp; + Create NAT Gateway <br>&emsp; + EC2 Instance Connect Endpoint | 17/09/2025   | 17/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - **Học lý thuyết:** Module 02-03 VPN - DirectConnect - LoadBalancer <br> - **Thực hành Hybrid DNS với Route 53:** <br>&emsp; + Generate Key Pair <br>&emsp; + Initialize CloudFormation <br>&emsp; + Configure Security Group <br>&emsp; + Connect to RDGW | 18/09/2025   | 18/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - **Thực hành Route 53 Resolver:** <br>&emsp; + Create Outbound Endpoint <br>&emsp; + Create Resolver Rules <br>&emsp; + Create Inbound Endpoints <br>&emsp; + Test DNS resolution <br> - **VPC Peering lab:** Setup và test VPC peering | 19/09/2025   | 19/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - **Thực hành VPC Peering:** <br>&emsp; + Create peering connection <br>&emsp; + Configure Route tables <br>&emsp; + Enable Cross-Peer DNS <br> - **Transit Gateway:** <br>&emsp; + Create TGW và attachments <br>&emsp; + Configure route tables <br> - **Clean up resources** | 20/09/2025   | 20/09/2025      | <https://cloudjourney.awsstudygroup.com/> |


### Kết quả đạt được tuần 2:

#### **📚 Kiến thức lý thuyết đã nắm được:**

* **AWS Virtual Private Cloud (VPC) cơ bản:**
  * Hiểu khái niệm VPC và tầm quan trọng trong AWS architecture
  * Nắm được các thành phần chính: Subnets, Route Tables, Internet Gateway
  * Biết cách thiết kế và quy hoạch IP addressing cho VPC
  * Hiểu về public vs private subnets và use cases

* **VPC Security và Multi-VPC features:**
  * Nắm vững Security Groups vs Network ACLs
  * Hiểu về VPC Flow Logs và monitoring
  * Biết các best practices về VPC security
  * Tìm hiểu về Multi-AZ và Multi-VPC architectures

* **Advanced Networking Services:**
  * Hiểu về VPN connections và use cases
  * Tìm hiểu AWS Direct Connect cho hybrid connectivity
  * Nắm được các loại Load Balancers và cách hoạt động
  * Biết về NAT Gateway vs NAT Instance

#### **🛠️ Kỹ năng thực hành đã đạt được:**

* **VPC Infrastructure Setup:**
  * Tạo thành công VPC với custom CIDR blocks
  * Thiết lập public và private subnets
  * Cấu hình Internet Gateway và Route Tables
  * Tạo và quản lý Security Groups với proper rules

* **EC2 trong VPC Environment:**
  * Deploy EC2 instances trong different subnets
  * Test connectivity giữa các instances
  * Thiết lập NAT Gateway cho private subnet internet access
  * Sử dụng EC2 Instance Connect Endpoint

* **Hybrid DNS với Route 53 Resolver:**
  * Generate và quản lý Key Pairs
  * Sử dụng CloudFormation templates
  * Cấu hình Security Groups cho hybrid connectivity
  * Setup Route 53 Outbound/Inbound Endpoints
  * Tạo và test DNS resolver rules

* **VPC Connectivity Patterns:**
  * **VPC Peering:**
    * Tạo và quản lý peering connections
    * Cấu hình route tables cho cross-VPC communication
    * Enable cross-peer DNS resolution
    * Update Network ACLs cho peering traffic

  * **Transit Gateway:**
    * Tạo Transit Gateway và attachments
    * Thiết lập TGW route tables
    * Integrate với VPC route tables
    * Test multi-VPC connectivity

#### **🔧 Tools và Technologies đã sử dụng:**
* AWS VPC Console và CLI
* CloudFormation templates
* Route 53 Resolver
* EC2 Instance Connect
* NAT Gateway
* Internet Gateway
* Transit Gateway
* VPC Peering

#### **💡 Insights và Best Practices học được:**

* **Network Design:**
  * Proper CIDR planning để tránh IP conflicts
  * Separation of concerns với public/private subnets
  * Multi-AZ deployment cho high availability

* **Security:**
  * Principle of least privilege trong Security Groups
  * Network segmentation với NACLs
  * Proper IAM roles cho network resources

* **Connectivity:**
  * Khi nào dùng VPC Peering vs Transit Gateway
  * Cost optimization cho NAT Gateway vs NAT Instance
  * DNS resolution strategies trong hybrid environments

* **Troubleshooting:**
  * Sử dụng VPC Flow Logs để debug connectivity
  * Common networking issues và cách resolve
  * Testing methodology cho network configurations

#### **🎯 Mức độ hoàn thành:**
* **Lý thuyết:** 100% - Hoàn thành modules 02-01, 02-02, 02-03
* **Thực hành:** 100% - Hoàn thành tất cả labs từ 02-Lab03 đến 02-Lab20
* **Overall:** Đạt được mục tiêu advanced networking cho tuần 2

#### **🚀 Chuẩn bị cho tuần tiếp theo:**
* Tìm hiểu sâu hơn về AWS Storage services (S3, EBS, EFS, FSx)
* Học về hybrid storage solutions (Storage Gateway, Snow Family)
* Thực hành VM migration và AWS Backup strategies
* Khám phá S3 advanced features (versioning, replication, lifecycle policies)
* Chuẩn bị môi trường VMware Workstation cho migration labs


