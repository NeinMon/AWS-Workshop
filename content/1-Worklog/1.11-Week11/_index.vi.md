---
title: "Worklog Tuần 11"
date: "2024-11-25T00:00:00Z"
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---

### Mục tiêu tuần 11:

* Triển khai Admin Management APIs (Deactivate Class, Deactivate User, Audit Logs)
* Tiến hành integration testing cho tất cả 14 APIs (Student + Admin)
* Tạo API documentation với Swagger/OpenAPI
* Chuẩn bị staging environment

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 2 | - Triển khai Admin API (Phần 1): <br>&emsp; + PATCH `/admin/classes/{id}` - Vô Hiệu Hóa Lớp <br>&emsp; + Validate admin role <br>&emsp; + Soft delete (status=0) <br>&emsp; + Trigger audit log | 18/11/2025 | 18/11/2025 | [API Gateway Authorization](https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-authorization-flow.html) |
| 3 | - Triển khai Admin API (Phần 2): <br>&emsp; + PATCH `/admin/users/{id}` - Vô Hiệu Hóa User <br>&emsp; + Update status trong Users table <br>&emsp; + Disable Cognito user <br>&emsp; + Trigger audit log | 19/11/2025 | 19/11/2025 | [Cognito Admin APIs](https://docs.aws.amazon.com/cognito/latest/developerguide/user-pool-settings-admin-apis.html) |
| 4 | - Triển khai Admin API (Phần 3): <br>&emsp; + GET `/admin/audit-logs` - Lấy Audit Logs <br>&emsp; + Query ActivityLogs table <br>&emsp; + Filter theo user_id, class_id, timestamp <br>&emsp; + Hỗ trợ phân trang | 20/11/2025 | 20/11/2025 | [DynamoDB GSI Queries](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/GSI.html) |
| 5 | - Integration testing: <br>&emsp; + Test tất cả 14 APIs (10 Student + 3 Admin + 1 Delete Comment) <br>&emsp; + Test admin authorization <br>&emsp; + Test audit logging <br>&emsp; + Load testing với 100 concurrent users | 21/11/2025 | 21/11/2025 | [Postman Collections](https://learning.postman.com/docs/collections/collections-overview/) |
| 6 | - API Documentation & Staging: <br> - Generate OpenAPI 3.0 specification <br> - Deploy Swagger UI <br> - Deploy lên staging environment <br> - Validate tất cả services | 22/11/2025 | 22/11/2025 | [OpenAPI Specification](https://swagger.io/specification/) |

### Kết quả đạt được tuần 11:

**DynamoDB Table cho Audit Logs**:
* ✅ Tạo **ActivityLogs** table:
  * PK: log_id (UUID)
  * Attributes: user_id, class_id, action_type, details (JSON), timestamp
  * GSI: user_id-index (PK=user_id, SK=timestamp) để filter theo user
  * GSI: class_id-index (PK=class_id, SK=timestamp) để filter theo class
  * TTL: expires_at (tự động xóa logs sau 365 ngày)

**APIs Đã Triển Khai (3 Admin Management Endpoints)**:
* ✅ **PATCH** `/admin/classes/{id}` - Vô Hiệu Hóa Lớp
  * Path parameter: class_id
  * Validate user có Admin role (từ JWT token)
  * Update Classes table: SET status = 0 (soft delete)
  * Giữ dữ liệu lịch sử (enrollments, assignments, submissions)
  * Trigger audit log: `{ action_type: "class_deactivated", class_id, admin_id, timestamp }`
  * Returns: `{ message: "Deleted" }`
  * Latency: ~80ms
  
* ✅ **PATCH** `/admin/users/{id}` - Vô Hiệu Hóa User
  * Path parameter: user_id
  * Validate user có Admin role
  * **Bước 1**: Update Users table: SET status = 0
  * **Bước 2**: Disable user trong Cognito User Pool bằng AdminDisableUser API
  * User không thể login sau khi bị vô hiệu hóa
  * Trigger audit log: `{ action_type: "user_deactivated", user_id, admin_id, timestamp }`
  * Returns: `{ message: "Deleted" }`
  * Latency: ~120ms (DynamoDB + Cognito)
  
* ✅ **GET** `/admin/audit-logs` - Lấy Audit Logs
  * Query parameters:
    - `user_id` (tùy chọn): Filter theo user
    - `class_id` (tùy chọn): Filter theo class
    - `timestamp` (tùy chọn): Filter theo khoảng thời gian
    - `limit` (mặc định: 100): Số lượng kết quả
  * Query ActivityLogs table:
    - Nếu có user_id: Sử dụng user_id-index GSI
    - Nếu có class_id: Sử dụng class_id-index GSI
    - Ngược lại: Scan table (giới hạn 7 ngày gần nhất)
  * Returns: `{ results: [{ log_id, user_id, class_id, action_type, details, timestamp }] }`
  * Phân trang: LastEvaluatedKey cho trang tiếp theo
  * Latency: ~200ms (GSI query), ~1.5s (full scan)

**Authorization & Security**:
* ✅ **Xác Thực Admin Role**:
  * Lambda authorizer kiểm tra JWT token cho `cognito:groups` claim
  * Chỉ users trong nhóm "Admin" mới truy cập được `/admin/*` endpoints
  * Returns 403 Forbidden nếu không phải admin
  
* ✅ **Audit Logging Đã Triển Khai**:
  * Tất cả hành động admin được log vào ActivityLogs table
  * Loại action: class_deactivated, user_deactivated, class_created, enrollment_added
  * Details lưu dưới dạng JSON: `{ old_value, new_value, reason }`
  * Hỗ trợ compliance và security tracking

**Integration Testing**:
* ✅ **Tạo Test Suite**: 35+ integration tests
  * Student APIs: 10 tests (submit, enroll, notifications, search, ranking)
  * Admin APIs: 6 tests (deactivate class, deactivate user, audit logs)
  * Post/Comment APIs: 8 tests (create post, list posts, create comment, delete post, delete comment)
  * Chat APIs: 4 tests (send message, get messages, reactions, delete comment)
  * Authentication: 6 tests (JWT validation, role-based access)
  
* ✅ **Test Coverage**: Tất cả 14 APIs đã được validate
  * Create, Read, Update, Delete operations
  * Error handling (400, 401, 403, 404, 500)
  * Edge cases (dữ liệu rỗng, input không hợp lệ, concurrent requests)
  * Kiểm tra authorization (Student không thể truy cập Admin APIs)
  
* ✅ **Kết Quả Load Testing**:
  * Tool: Apache JMeter
  * Cấu hình: 100 concurrent users, 10 phút
  * Kết quả:
    - Response time trung bình: 220ms
    - 95th percentile: 580ms
    - Error rate: 0.08%
    - Throughput: 450 requests/second
    - Admin APIs: 180ms trung bình (audit log query: 250ms)

**API Documentation**:
* ✅ **OpenAPI 3.0 Specification**:
  * 14 endpoints được document đầy đủ (10 Student + 3 Admin + 1 Delete Comment)
  * Request/response schemas
  * Yêu cầu authentication (JWT + role-based)
  * Error codes và messages
  * Ví dụ requests/responses
  
* ✅ **Swagger UI Đã Deploy**:
  * URL: https://api-docs.student-mgmt.com
  * Interactive API testing
  * Code generation (JavaScript, Python, cURL)
  * Phân chia thành các sections: Student APIs, Admin APIs, Chat APIs
  
* ✅ **Tạo Integration Guide**:
  * Authentication flow (Cognito JWT)
  * Role-based access control (Student, Teacher, Admin)
  * Cách sử dụng API endpoints
  * Quy trình upload/download files
  * Rate limiting và best practices

**Staging Deployment**:
* ✅ Staging environment đã deploy:
  * API Gateway: https://staging-api.student-mgmt.com
  * Tất cả Lambda functions deployed (17 functions)
  * DynamoDB tables với test data (9 tables)
  * S3 buckets đã cấu hình
  * Cognito user pool với test users (10 students, 3 teachers, 2 admins)
  * REST APIs cho chat với polling mechanism
  
* ✅ Staging validation:
  * Tất cả 14 APIs hoạt động bình thường
  * Chat polling mechanism hoạt động
  * Audit logging đã được xác minh
  * Admin deactivation đã test (class + user)
  * Performance: 85% code coverage
  * Lambda cold start: <1.5s (warm: <200ms)
  * DynamoDB read/write latency: <50ms
  * S3 upload/download speed: >10MB/s
  * Email notification delivery: <30s

**Tools & Technologies Sử Dụng**:
* **API Development**: AWS Lambda, API Gateway
* **Database**: Amazon DynamoDB với 9 tables
* **Authentication**: Amazon Cognito User Pools với JWT
* **Storage**: Amazon S3 cho file storage
* **Monitoring**: CloudWatch Logs, CloudWatch Metrics, X-Ray tracing
* **Testing**: Postman/Newman, JMeter (load testing), Jest (unit tests)
* **Documentation**: Swagger/OpenAPI 3.0, API Blueprint
* **CI/CD**: AWS CodePipeline, CodeBuild, CodeDeploy
* **IaC**: AWS CDK (TypeScript) hoặc CloudFormation
* **Development**: VS Code, AWS Toolkit, CloudShell


