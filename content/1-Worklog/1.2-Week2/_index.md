---
title: "Week 2 Worklog"
date: "2024-01-01T00:00:00Z"
weight: 1
chapter: false
pre: " <b> 1.2. </b> "
---

### Week 2 Objectives:

* Deep dive into AWS VPC and networking services
* Practice creating and configuring VPC infrastructure
* Learn about VPC Security, VPN, DirectConnect, and Load Balancer
* Practice complex networking scenarios

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                   | Start Date | Completion Date | Reference Material                            |
| --- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ----------------------------------------- |
| 2   | - **Theory:** Module 02-01 AWS Virtual Private Cloud <br> - **Basic VPC Practice:** <br>&emsp; + Create VPC <br>&emsp; + Create Subnet <br>&emsp; + Create Internet Gateway <br>&emsp; + Create Route Table <br>&emsp; + Create Security Groups | 16/09/2025   | 16/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 3   | - **Theory:** Module 02-02 VPC Security and Multi-VPC features <br> - **EC2 in VPC Practice:** <br>&emsp; + Create EC2 Instances in Subnets <br>&emsp; + Test connection <br>&emsp; + Create NAT Gateway <br>&emsp; + EC2 Instance Connect Endpoint | 17/09/2025   | 17/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - **Theory:** Module 02-03 VPN - DirectConnect - LoadBalancer <br> - **Hybrid DNS with Route 53 Practice:** <br>&emsp; + Generate Key Pair <br>&emsp; + Initialize CloudFormation <br>&emsp; + Configure Security Group <br>&emsp; + Connect to RDGW | 18/09/2025   | 18/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - **Route 53 Resolver Practice:** <br>&emsp; + Create Outbound Endpoint <br>&emsp; + Create Resolver Rules <br>&emsp; + Create Inbound Endpoints <br>&emsp; + Test DNS resolution <br> - **VPC Peering lab:** Setup and test VPC peering | 19/09/2025   | 19/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - **VPC Peering Practice:** <br>&emsp; + Create peering connection <br>&emsp; + Configure Route tables <br>&emsp; + Enable Cross-Peer DNS <br> - **Transit Gateway:** <br>&emsp; + Create TGW and attachments <br>&emsp; + Configure route tables <br> - **Clean up resources** | 20/09/2025   | 20/09/2025      | <https://cloudjourney.awsstudygroup.com/> |


### Week 2 Achievements:

#### **📚 Theoretical Knowledge Acquired:**

* **AWS Virtual Private Cloud (VPC) Basics:**
  * Understand VPC concept and its importance in AWS architecture
  * Know the main components: Subnets, Route Tables, Internet Gateway
  * Learn how to design and plan IP addressing for VPC
  * Understand public vs private subnets and use cases

* **VPC Security and Multi-VPC Features:**
  * Master Security Groups vs Network ACLs
  * Understand VPC Flow Logs and monitoring
  * Know best practices for VPC security
  * Learn about Multi-AZ and Multi-VPC architectures

* **Advanced Networking Services:**
  * Understand VPN connections and use cases
  * Learn AWS Direct Connect for hybrid connectivity
  * Know Load Balancer types and how they work
  * Understand NAT Gateway vs NAT Instance

#### **🛠️ Practical Skills Achieved:**

* **VPC Infrastructure Setup:**
  * Successfully created VPC with custom CIDR blocks
  * Set up public and private subnets
  * Configured Internet Gateway and Route Tables
  * Created and managed Security Groups with proper rules

* **EC2 in VPC Environment:**
  * Deployed EC2 instances in different subnets
  * Tested connectivity between instances
  * Set up NAT Gateway for private subnet internet access
  * Used EC2 Instance Connect Endpoint

* **Hybrid DNS with Route 53 Resolver:**
  * Generated and managed Key Pairs
  * Used CloudFormation templates
  * Configured Security Groups for hybrid connectivity
  * Set up Route 53 Outbound/Inbound Endpoints
  * Created and tested DNS resolver rules

* **VPC Connectivity Patterns:**
  * **VPC Peering:**
    * Created and managed peering connections
    * Configured route tables for cross-VPC communication
    * Enabled cross-peer DNS resolution
    * Updated Network ACLs for peering traffic

  * **Transit Gateway:**
    * Created Transit Gateway and attachments
    * Set up TGW route tables
    * Integrated with VPC route tables
    * Tested multi-VPC connectivity

#### **🔧 Tools and Technologies Used:**
* AWS VPC Console and CLI
* CloudFormation templates
* Route 53 Resolver
* EC2 Instance Connect
* NAT Gateway
* Internet Gateway
* Transit Gateway
* VPC Peering

#### **💡 Insights and Best Practices Learned:**

* **Network Design:**
  * Proper CIDR planning to avoid IP conflicts
  * Separation of concerns with public/private subnets
  * Multi-AZ deployment for high availability

* **Security:**
  * Principle of least privilege in Security Groups
  * Network segmentation with NACLs
  * Proper IAM roles for network resources

* **Connectivity:**
  * When to use VPC Peering vs Transit Gateway
  * Cost optimization for NAT Gateway vs NAT Instance
  * DNS resolution strategies in hybrid environments

* **Troubleshooting:**
  * Using VPC Flow Logs to debug connectivity
  * Common networking issues and how to resolve them
  * Testing methodology for network configurations

#### **🎯 Completion Level:**
* **Theory:** 100% - Completed modules 02-01, 02-02, 02-03
* **Practice:** 100% - Completed all labs from 02-Lab03 to 02-Lab20
* **Overall:** Achieved advanced networking goals for week 2

#### **🚀 Preparation for Next Week:**
* Learn more about AWS Storage services (S3, EBS, EFS, FSx)
* Study hybrid storage solutions (Storage Gateway, Snow Family)
* Practice VM migration and AWS Backup strategies
* Explore S3 advanced features (versioning, replication, lifecycle policies)
* Prepare VMware Workstation environment for migration labs
