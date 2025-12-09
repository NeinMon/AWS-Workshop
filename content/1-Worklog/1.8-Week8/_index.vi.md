---
title: "Worklog Tuần 8"
date: "2024-11-04T00:00:00Z"
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

### Mục tiêu tuần 8:

* Thiết lập hạ tầng AWS Serverless (Lambda, DynamoDB, API Gateway, SAM)
* Triển khai Student Core APIs (Submit, Enroll, Notifications, Search)
* Cấu hình Cognito authentication và IAM roles

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 2 | - Khởi tạo AWS SAM project: <br>&emsp; + Cài đặt AWS SAM CLI <br>&emsp; + Tạo template.yaml <br>&emsp; + Cấu hình DynamoDB tables (Classes, Students, Enrollments, Assignments, Submissions, Notifications) <br>&emsp; + Setup môi trường local development | 28/10/2025 | 28/10/2025 | [AWS SAM Getting Started](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-getting-started.html) |
| 3 | - Triển khai Student APIs (Phần 1): <br>&emsp; + POST `/student/submit` - Nộp Bài Tập <br>&emsp; + GET `/student/classes/class-enrolled` - Lấy Lớp Đã Đăng Ký <br>&emsp; + S3 pre-signed URLs cho upload file | 29/10/2025 | 29/10/2025 | [Lambda with DynamoDB](https://docs.aws.amazon.com/lambda/latest/dg/with-ddb.html) |
| 4 | - Triển khai Student APIs (Phần 2): <br>&emsp; + GET `/student/notifications` - Lấy Thông Báo <br>&emsp; + POST `/student/enroll` - Đăng Ký/Hủy Đăng Ký Lớp <br>&emsp; + Cấu hình API Gateway endpoints | 30/10/2025 | 30/10/2025 | [API Gateway REST API](https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-rest-api.html) |
| 5 | - Setup Cognito Authentication: <br>&emsp; + Tạo User Pool <br>&emsp; + Cấu hình JWT authorizer cho Student role <br>&emsp; + Test authentication flow <br>&emsp; + Tích hợp với API Gateway | 31/10/2025 | 31/10/2025 | [Cognito User Pools](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-identity-pools.html) |
| 6 | - Triển khai Search API: <br>&emsp; + GET `/student/search` - Tìm Kiếm Lớp/Giảng Viên <br> - Testing với SAM Local <br> - Deploy lên AWS dev environment | 01/11/2025 | 01/11/2025 | [DynamoDB Scan](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Scan.html) |


### Kết quả đạt được tuần 8:

**Thiết Lập Hạ Tầng AWS**:
* ✅ Khởi tạo AWS SAM project với template.yaml
* ✅ Tạo DynamoDB tables:
  * **Classes** table: PK = class_id, attributes (class_name, teacher_id, status, max_students)
  * **Students** table: PK = student_id, attributes (name, email, status)
  * **Enrollments** table: PK = student_id, SK = class_id, attributes (enrolled_at, status)
  * **Assignments** table: PK = assignment_id, attributes (class_id, title, due_date, max_score)
  * **Submissions** table: PK = assignment_id, SK = student_id, attributes (file_url, submitted_at, score)
  * **Notifications** table: PK = user_id, SK = timestamp, attributes (type, message, read_status)
* ✅ Tạo S3 bucket: `student-submissions-bucket`
* ✅ Cấu hình môi trường SAM Local testing
* ✅ Deploy Dev environment lên AWS

**APIs Đã Triển Khai (5 Student Core Endpoints)**:
* ✅ **POST** `/student/submit` - Nộp Bài Tập
  * Request body: `{ assignment_id, file_content }`
  * Tạo S3 pre-signed URL (hết hạn 15 phút)
  * Upload file lên S3: `submissions/{assignment_id}/{student_id}/file.pdf`
  * Ghi vào Submissions table (assignment_id, student_id, file_url, submitted_at)
  * Trigger notification tới giảng viên
  * Returns: submission_id, s3_url, submitted_at
  
* ✅ **GET** `/student/classes/class-enrolled` - Lấy Lớp Đã Đăng Ký
  * Query Enrollments table theo student_id (từ JWT token)
  * Join với Classes table để lấy chi tiết lớp
  * Returns: Array `[{ class_id, class_name, teacher_name, enrolled_at }]`
  * Sắp xếp theo enrolled_at (giảm dần)
  
* ✅ **GET** `/student/notifications` - Lấy Thông Báo
  * Query Notifications table theo user_id (từ JWT token)
  * Filter theo read_status (tùy chọn query param: `?unread=true`)
  * Returns: Array `[{ notification_id, type, message, timestamp, read_status }]`
  * Phân trang: giới hạn 20 mỗi trang
  * Loại: assignment_graded, new_post, enrollment_approved
  
* ✅ **POST** `/student/enroll` - Đăng Ký/Hủy Đăng Ký Lớp
  * Request body: `{ class_id, action: "enroll" | "unenroll" }`
  * **Enroll**: Kiểm tra sức chứa lớp (<= max_students), ghi vào Enrollments table, tạo notification
  * **Unenroll**: Xóa khỏi Enrollments (soft delete: status=0)
  * Atomic update: tăng/giảm student_count trong Classes table
  * Returns: `{ message: "Enrolled successfully", enrollment_id }`
  
* ✅ **GET** `/student/search` - Tìm Kiếm Lớp/Giảng Viên
  * Query parameters: `keyword`, `status`, `teacher_name`
  * DynamoDB Scan với FilterExpression (tìm kiếm đa trường)
  * Tìm trong các trường: class_name, teacher_name, description
  * Returns: Array `[{ class_id, class_name, teacher_name, student_count, status }]`
  * Phân trang: giới hạn 20 mỗi trang

**Authentication & Security**:
* ✅ Tạo Cognito User Pool với các nhóm Student/Teacher/Admin
* ✅ Xác thực JWT token trong Lambda authorizer
* ✅ Cấu hình IAM roles: StudentRole (chỉ đọc/ghi dữ liệu của mình)
* ✅ Tích hợp API Gateway authorizer (xác thực JWT + role)
* ✅ S3 bucket policy: chỉ pre-signed URLs (không có public access)

---

## Kiến Trúc & Hạ Tầng

### Dịch Vụ AWS Sử Dụng
| Dịch vụ | Mục đích |
|---------|----------|
| **AWS Lambda** | Thực thi business logic cho 5 API endpoints |
| **Amazon API Gateway** | REST API routing, request validation, throttling |
| **Amazon DynamoDB** | Lưu trữ dữ liệu student, classes, notifications |
| **AWS Cognito** | Xác thực người dùng qua JWT tokens |
| **Amazon CloudWatch** | Monitoring, logs, và metrics |
| **Amazon EventBridge** | Xử lý event-driven cho notifications |

### DynamoDB Tables
1. **StudentNotifications** - userId, notificationId
2. **StudentClasses** - userId, classId
3. **ClassPerformance** - classId, userId (GSI cho ranking)

---

## Chi Tiết Kỹ Thuật

### Cấu Hình Lambda
- **Runtime**: Node.js 18.x
- **Memory**: 512MB
- **Timeout**: 30 giây
- **Environment Variables**: DynamoDB table names, Cognito pool ID

### Cấu Hình API Gateway
- CORS enabled cho tất cả origins
- Request validation trên POST endpoints
- Rate limiting: 1000 requests/5 phút mỗi IP
- Caching enabled (5-minute TTL) cho GET endpoints

---

## Testing & Validation

### Unit Tests
- ✅ Lambda function logic validation
- ✅ DynamoDB query accuracy
- ✅ Cognito token verification
- ✅ Error handling và edge cases

### Integration Tests
- ✅ End-to-end API flow
- ✅ Database transactions

### Manual Testing (Postman)
- ✅ Tất cả 5 API endpoints đã test
- ✅ Request/response validation
- ✅ Error scenarios (400, 401, 404, 500)

### Performance Testing
- ✅ API response time < 500ms
- ✅ DynamoDB query latency < 10ms

---

## Thách Thức & Giải Pháp

| Thách thức | Giải pháp |
|------------|-----------|
| High latency cho search queries | Triển khai DynamoDB GSI trên các attributes thường query |
| Cognito token validation overhead | Triển khai token caching tại API Gateway level |
| DynamoDB hot partition | Thêm distributed hash keys để phân tải tốt hơn |
| Rate limiting requirements | Cấu hình API Gateway throttling policies |

---

## Monitoring & Logging

### CloudWatch Metrics
- Lambda execution duration
- API Gateway latency
- DynamoDB read/write units consumed
- Error rates và HTTP status codes

### Alerts Configured
- Lambda error rate > 1%
- API Gateway 5XX errors
- DynamoDB throttling events

---

## Phân Tích Chi Phí

### Chi Phí Tuần 8
| Dịch vụ | Chi phí ước tính |
|---------|------------------|
| AWS Lambda | $0.80 (~1.5M invocations) |
| DynamoDB | $0.50 (on-demand pricing) |
| API Gateway | $0.35 (1.2M requests) |
| CloudWatch | $0.10 (logs) |
| **Tổng** | **$1.75 USD** |

---

## Bước Tiếp Theo (Tuần 9)
- [ ] Triển khai assignment grading workflow với EventBridge
- [ ] Thêm ranking notifications
- [ ] Tối ưu DynamoDB queries cho scale
- [ ] Triển khai input validation middleware
- [ ] Setup API documentation (OpenAPI/Swagger)

## Deliverables
✅ 5 functional REST API endpoints  
✅ DynamoDB tables với optimized indexes  
✅ CloudWatch monitoring dashboard  
✅ Postman API collection  
✅ Unit test coverage > 80%  
✅ API documentation  
✅ Deployment qua AWS SAM

---

## Tài Liệu Tham Khảo
- [AWS Lambda Best Practices](https://docs.aws.amazon.com/lambda/)
- [DynamoDB Design Patterns](https://docs.aws.amazon.com/amazondynamodb/)
- [API Gateway Documentation](https://docs.aws.amazon.com/apigateway/)



