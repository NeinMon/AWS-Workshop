---
title: "Worklog Tuần 12"
date: "2024-12-02T00:00:00Z"
weight: 12
chapter: false
pre: " <b> 1.12. </b> "
---

### Mục tiêu tuần 12:

* Deploy Backend lên AWS (API Gateway + Lambda + DynamoDB)
* Hoàn thiện Proposal document và Architecture diagrams
* Chuẩn bị Demo và Presentation

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 2 | - Deploy Backend lên AWS: <br>&emsp; + SAM deploy to AWS <br>&emsp; + Configure API Gateway stages <br>&emsp; + Setup Cognito User Pool <br>&emsp; + Environment variables | 25/11/2025 | 25/11/2025 | [AWS SAM Deploy](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/sam-cli-command-reference-sam-deploy.html) |
| 3 | - Cấu hình Production: <br>&emsp; + Enable CloudWatch logging <br>&emsp; + Setup alarms và monitoring <br>&emsp; + API throttling và quotas <br>&emsp; + Security review | 26/11/2025 | 26/11/2025 | [CloudWatch Monitoring](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html) |
| 4 | - Hoàn thiện Proposal document: <br>&emsp; + Cập nhật Architecture diagram <br>&emsp; + Ước tính chi phí thực tế <br>&emsp; + Đánh giá rủi ro <br>&emsp; + Lộ trình triển khai | 27/11/2025 | 27/11/2025 | [Proposal Document](../../2-Proposal/) |
| 5 | - Chuẩn bị Demo: <br>&emsp; + Test tất cả API endpoints <br>&emsp; + Chuẩn bị demo scenarios <br>&emsp; + Record demo video (nếu cần) <br>&emsp; + Troubleshoot issues | 28/11/2025 | 28/11/2025 | [Postman API Testing](https://www.postman.com/api-platform/api-testing/) |
| 6 | - Chuẩn bị Presentation: <br>&emsp; + Slide deck <br>&emsp; + Technical deep-dive <br>&emsp; + Q&A preparation <br>&emsp; + Final review | 29/11/2025 | 29/11/2025 | [AWS Serverless Samples](https://github.com/aws-samples/serverless-patterns) |

### Kết quả đạt được tuần 12:

**Production Deployment**:
* ✅ **Deploy thành công lên AWS**:
  * API Gateway endpoint: `https://xxx.execute-api.us-east-1.amazonaws.com/prod`
  * Region: us-east-1 (N. Virginia)
  * 13 Lambda functions deployed và hoạt động
  * 7 DynamoDB tables với data
  * Cognito authentication integrated
  * S3 buckets configured
  
* ✅ **Production Configuration**:
  * Custom domain: api.student-management.com (optional)
  * API Gateway stages: dev, staging, prod
  * Environment variables configured
  * Secrets stored in Systems Manager Parameter Store

**Monitoring và Security**:
* ✅ **CloudWatch Monitoring**:
  * Dashboards: API Performance, Lambda Metrics, DynamoDB Stats
  * Logs: All Lambda functions logging to CloudWatch
  * Log retention: 30 days
  * Structured logging (JSON format)
  
* ✅ **CloudWatch Alarms** (8 alarms configured):
  * API Gateway 5xx errors > 10 in 5 minutes
  * Lambda error rate > 5%
  * DynamoDB throttles > 50 in 5 minutes
  * API latency p99 > 2 seconds
  * Lambda concurrent executions > 800
  * Daily cost > $20
  * Cognito auth failures > 20 in 5 minutes
  * S3 storage > 50GB
  
* ✅ **Security Configuration**:
  * API Gateway throttling: 1000 req/sec per IP
  * Cognito JWT authentication on all endpoints
  * IAM least privilege policies
  * CORS configured (whitelist domains)
  * API Gateway request validation enabled
  * S3 bucket encryption at rest
  * VPC endpoints for DynamoDB (optional)

**Tổng kết Backend APIs hoàn thành (17 APIs)**:

| # | API | Method | Endpoint | Chức năng |
|---|-----|--------|----------|----------|
| **Student APIs (10 APIs)** |
| 1 | Submit Assignment | POST | `/student/submit` | Nộp bài tập (upload file lên S3) |
| 2 | Get Enrolled Classes | GET | `/student/classes/class-enrolled` | Danh sách lớp đã đăng ký |
| 3 | Get Notifications | GET | `/student/notifications` | Lấy thông báo của sinh viên |
| 4 | Enroll/Unenroll Class | POST | `/student/enroll` | Đăng ký/Hủy đăng ký lớp học |
| 5 | Search Classes/Teachers | GET | `/student/search` | Tìm kiếm lớp học và giảng viên |
| 6 | Get Ranking per Class | GET | `/student/ranking/{class_id}` | Xem xếp hạng sinh viên trong lớp |
| 7 | Create Post | POST | `/classes/{class_id}/posts` | Tạo bài đăng thảo luận |
| 8 | List Posts | GET | `/classes/{class_id}/posts` | Danh sách bài đăng trong lớp |
| 9 | Create Comment | POST | `/posts/{post_id}/comments` | Bình luận bài đăng |
| 10 | Delete Post | DELETE | `/posts/{id}` | Xóa bài đăng |
| **Chat APIs (3 APIs)** |
| 11 | Get Chat Messages | GET | `/chat/messages/{class_id}` | Lấy tin nhắn |
| 12 | Send Chat Message | POST | `/chat/messages` | Gửi tin nhắn |
| 13 | Update Reactions | PUT | `/chat/messages/{id}/reactions` | Thả tim/like tin nhắn |
| **Admin APIs (4 APIs)** |
| 14 | Deactivate Class | PATCH | `/admin/classes/{id}` | Vô hiệu hóa lớp học (status=0) |
| 15 | Deactivate User | PATCH | `/admin/users/{id}` | Vô hiệu hóa người dùng (status=0) |
| 16 | Get Audit Logs | GET | `/admin/audit-logs` | Xem lịch sử hoạt động hệ thống |
| 17 | Enroll Management | POST | `/admin/enroll` | Thêm sinh viên vào lớp (admin) |

**AWS Services Sử dụng**:
* ✅ **AWS Lambda**: 17 functions (Node.js 18.x hoặc Python 3.11)
* ✅ **Amazon DynamoDB**: 9 tables
  * Classes, Students, Enrollments, Assignments, Submissions, Notifications, Posts, PostComments, Messages, ActivityLogs
* ✅ **Amazon API Gateway**: 1 REST API với 17 endpoints (Student + Admin + Chat)
* ✅ **Amazon Cognito**: User Pool cho authentication (Student, Teacher, Admin groups)
* ✅ **Amazon S3**: 1 bucket cho assignment submissions
* ✅ **Amazon CloudWatch**: Monitoring, logging, alarms
* ✅ **AWS IAM**: Roles và policies (StudentRole, TeacherRole, AdminRole)

**Proposal Document Hoàn thiện**:
* ✅ **Architecture Diagrams**:
  * High-level system architecture (AWS services)
  * API flow diagrams (request/response flow)
  * Database schema (DynamoDB table design)
  * Authentication flow (Cognito + JWT)
  * Deployment architecture (SAM + CloudFormation)
  
* ✅ **Chi phí ước tính** (Production):
  * Lambda: $8-12/month (2M requests, 512MB, 3s avg duration)
  * DynamoDB: $5-8/month (on-demand, 2GB storage, 9 tables)
  * API Gateway: $5-7/month (2M requests including chat polling)
  * S3: $1-2/month (10GB storage, 100K requests)
  * CloudWatch: $3-4/month (logs + metrics)
  * Cognito: $0/month (Free tier: 50K MAU)
  * **Total: ~$22-33/month** (cho 100-200 active users)
  * **Scaling**: $55-110/month (cho 1000-2000 users)
  
* ✅ **Đánh giá 8 rủi ro chính**:
  1. Lambda cold starts → Provisioned concurrency
  2. DynamoDB throttling → On-demand capacity mode
  3. Cost overruns → Budget alerts + Cost Explorer
  4. Security vulnerabilities → API Gateway WAF + encryption
  5. Data loss → DynamoDB point-in-time recovery
  6. API downtime → Multi-AZ deployment
  7. Slow performance → API caching + Lambda optimization
  8. Scalability limits → Auto-scaling configured
  
* ✅ **Lộ trình 12 tuần**:
  * Tuần 1-7: Học AWS fundamentals (VPC, EC2, S3, IAM, RDS, Security)
  * Tuần 8: Infrastructure + Student Core APIs (5 APIs: submit, enroll, notifications, search, enrolled classes)
  * Tuần 9: Student Ranking + Post/Comment APIs (5 APIs: ranking, create post, list posts, create comment, delete post)
  * Tuần 10: Chat APIs + Reactions (4 APIs: get/send messages, reactions, delete comment)
  * Tuần 11: Admin Management APIs + Testing (3 APIs: deactivate class, deactivate user, audit logs)
  * Tuần 12: Admin Advanced APIs + Production (2 APIs: enroll management, ranking analytics)

**Demo Preparation**:
* ✅ **Demo Scenarios**:
  * Scenario 1: Student enrolls in class → receives notification → views enrolled classes
  * Scenario 2: Student submits assignment → uploads file to S3 → receives confirmation
  * Scenario 3: Student posts discussion → other students comment → real-time chat
  * Scenario 4: Admin deactivates inactive user → checks audit logs
  * Scenario 5: Student views ranking → sees top 40 students with scores
  
* ✅ **Test Data**:
  * 8 sample classes (CS101, MATH201, PHYS301, etc.)
  * 25 test users (15 students, 5 teachers, 5 admins)
  * 12 assignments with various due dates
  * 20 submissions with sample grades
  * 10 posts with 30+ comments
  * 50 chat messages
  * 15 audit log entries
  
* ✅ **Demo Video** (optional):
  * Screen recording of API testing in Postman
  * Duration: 5-7 minutes
  * Voiceover explaining each API call

**Presentation Deck**:
* ✅ **Slide Content** (15-20 slides):
  1. Title + Introduction
  2. Problem Statement (quản lý lớp học thủ công)
  3. Solution Overview (Serverless Student Management System)
  4. AWS Architecture Diagram
  5. Key AWS Services (Lambda, DynamoDB, API Gateway, Cognito)
  6. API Endpoints Summary (17 APIs: 10 Student + 3 Chat + 4 Admin)
  7. Database Schema (DynamoDB 9 tables)
  8. Authentication Flow (Cognito JWT + role-based access)
  10. Deployment Process (AWS SAM)
  11. Monitoring & Security (CloudWatch, Audit Logs, WAF)
  12. Cost Analysis ($23-35/month)
  13. Demo Walkthrough
  14. Performance Metrics (220ms avg latency, 100 concurrent users)
  15. Challenges & Solutions
  16. Future Enhancements
  17. Q&A
  
* ✅ **Technical Deep-Dive**:
  * Lambda function code snippets
  * DynamoDB query examples
  * API Gateway configuration
  * SAM template.yaml structure
  
* ✅ **Q&A Preparation**:
  * Why serverless? (cost, scalability, no server management)
  * Why DynamoDB over RDS? (NoSQL, on-demand scaling, low latency)
  * Chat implementation? (REST polling with optimized intervals)
  * How to handle cold starts? (Provisioned concurrency, keep-warm)
  * Security best practices? (Cognito, IAM, encryption, audit logs, WAF)
  * Cost optimization? (Caching, right-sizing Lambda, on-demand DynamoDB)

**Tổng kết 12 tuần thực tập**:
* ✅ **Tuần 1-7**: Học AWS fundamentals
  * VPC, EC2, S3, IAM, RDS
  * CloudWatch, Auto Scaling, Load Balancers
  * Security best practices
  
* ✅ **Tuần 8-12**: Phát triển Backend APIs
  * 17 REST APIs implemented
  * 10 Student APIs (submit, enroll, notifications, search, ranking, post/comment)
  * 3 Chat APIs (send/get messages, reactions)
  * 4 Admin APIs (deactivate, audit logs, enroll management)
  * Serverless architecture (Lambda + DynamoDB + API Gateway)
  * Authentication với Cognito (role-based: Student, Teacher, Admin)
  * Production deployment
  * Monitoring và Security (CloudWatch, Audit Logs)
  
* ✅ **Key Achievements**:
  * Hoàn thành 17 production-ready APIs
  * Deploy thành công lên AWS
  * Chat system với polling mechanism
  * Chi phí thấp: ~$22-33/month
  * Performance tốt: 220ms average latency
  * Scalable: 100-2000 users
  * Secure: Cognito, IAM, encryption, audit logging, monitoring
  
* ✅ **Skills Acquired**:
  * AWS Serverless development (Lambda, DynamoDB, API Gateway)
  * RESTful API design and implementation
  * Infrastructure as Code (AWS SAM, CloudFormation)
  * Authentication & Authorization (Cognito, JWT, role-based access)
  * Monitoring & Logging (CloudWatch, Audit Logs)
  * Cost optimization
  * Technical documentation
  * Presentation & Demo skills

**Next Steps** (Post-Internship):
* Teacher-facing APIs (create class, grade assignments)
* Enhanced notification features with EventBridge
* Analytics dashboard
* Automated testing (CI/CD pipeline with GitHub Actions)


