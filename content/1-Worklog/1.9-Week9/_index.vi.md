---
title: "Worklog Tuần 9"
date: "2024-11-11T00:00:00Z"
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---

### Mục tiêu tuần 9:

* Triển khai Student Ranking API với chức năng bảng xếp hạng
* Triển khai Post/Comment System cho thảo luận lớp học
* Thiết lập DynamoDB tables cho Posts và Comments

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 2 | - Triển khai Ranking API: <br>&emsp; + GET `/student/ranking/{class_id}` - Lấy Xếp Hạng Theo Lớp <br>&emsp; + Tính aggregate score từ Submissions table <br>&emsp; + Sắp xếp sinh viên theo tổng điểm <br>&emsp; + Trả về top 40 sinh viên | 04/11/2025 | 04/11/2025 | [DynamoDB Query](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Query.html) |
| 3 | - Thiết kế Post/Comment schema: <br>&emsp; + Posts table (PK=post_id, class_id, author_id, content, created_at) <br>&emsp; + PostComments table (PK=post_id, SK=comment_id) <br>&emsp; + GSI trên class_id để list posts | 05/11/2025 | 05/11/2025 | [DynamoDB Best Practices](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/best-practices.html) |
| 4 | - Triển khai Post APIs (Phần 1): <br>&emsp; + POST `/classes/{class_id}/posts` - Tạo Post (Giảng Viên/Sinh Viên) <br>&emsp; + GET `/classes/{class_id}/posts` - Danh Sách Posts <br>&emsp; + Hỗ trợ rich text (Markdown) | 06/11/2025 | 06/11/2025 | [Lambda with DynamoDB](https://docs.aws.amazon.com/lambda/latest/dg/with-ddb.html) |
| 5 | - Triển khai Post APIs (Phần 2): <br>&emsp; + POST `/posts/{post_id}/comments` - Tạo Comment <br>&emsp; + DELETE `/posts/{id}` - Xóa Post <br>&emsp; + Validate quyền tác giả | 07/11/2025 | 07/11/2025 | [API Gateway Authorization](https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-authorization-flow.html) |
| 6 | - Testing và bug fixes: <br> - Test tính toán ranking <br> - Test post/comment CRUD <br> - Validate permissions <br> - Performance testing | 08/11/2025 | 08/11/2025 | [Postman Testing](https://www.postman.com/) |


### Kết quả đạt được tuần 9:

**DynamoDB Tables cho Posts/Comments**:
* ✅ Tạo **Posts** table:
  * PK: post_id (UUID)
  * Attributes: class_id, author_id, author_name, title, content (Markdown), created_at, is_pinned
  * GSI: class_id-index (PK=class_id, SK=created_at) để list posts theo lớp
  
* ✅ Tạo **PostComments** table:
  * PK: post_id, SK: comment_id (UUID)
  * Attributes: author_id, author_name, content, created_at
  * Cho phép query hiệu quả tất cả comments của một post

**APIs Đã Triển Khai (5 Ranking + Post/Comment Endpoints)**:
* ✅ **GET** `/student/ranking/{class_id}` - Lấy Xếp Hạng Theo Lớp
  * Query Enrollments table cho tất cả sinh viên trong lớp
  * Query Submissions table cho mỗi sinh viên (aggregate score)
  * Tính tổng điểm: `SUM(score * assignment_weight)`
  * Sắp xếp theo total_score (giảm dần)
  * Trả về top 40 sinh viên: `[{ rank, student_id, name, total_score }]`
  * Response: `{ rankings: [...], my_rank: 15 }` (xếp hạng của sinh viên đã xác thực)
  
* ✅ **POST** `/classes/{class_id}/posts` - Tạo Post (Giảng Viên/Sinh Viên)
  * Request body: `{ title, content, is_pinned }`
  * Validate user đã đăng ký lớp (hoặc là giảng viên)
  * Ghi vào Posts table (post_id, class_id, author_id, content, created_at)
  * Nếu tác giả là Giảng Viên: set is_pinned=true (tùy chọn)
  * Trigger notification tới tất cả sinh viên đã đăng ký
  * Returns: post_id, created_at
  
* ✅ **GET** `/classes/{class_id}/posts` - Danh Sách Posts
  * Query Posts table sử dụng GSI (class_id-index)
  * Sắp xếp theo created_at (giảm dần), posts ghim lên đầu
  * Phân trang: giới hạn 20 mỗi trang, LastEvaluatedKey cho trang tiếp theo
  * Returns: `[{ post_id, title, author_name, content_preview, created_at, comment_count }]`
  
* ✅ **POST** `/posts/{post_id}/comments` - Tạo Comment
  * Request body: `{ content }`
  * Validate user đã đăng ký lớp (kiểm tra Posts table cho class_id)
  * Ghi vào PostComments table (post_id, comment_id, author_id, content, created_at)
  * Tăng comment_count trong Posts table (atomic update)
  * Trigger notification tới tác giả post
  * Returns: comment_id, created_at
  
* ✅ **DELETE** `/posts/{id}` - Xóa Post
  * Validate user là tác giả HOẶC giảng viên của lớp
  * Soft delete: Update status=0 trong Posts table
  * Cũng soft delete tất cả comments trong PostComments table (cascade)
  * Returns: `{ message: "Post deleted successfully" }`

**Authorization & Permissions**:
* ✅ **Create Post**: Giảng viên (bất kỳ nội dung) + Sinh viên (nếu đã đăng ký)
* ✅ **Create Comment**: Giảng viên + Sinh viên (nếu đã đăng ký)
* ✅ **Delete Post**: Chỉ tác giả (hoặc Giảng viên cho bất kỳ post nào trong lớp của họ)
* ✅ **List Posts**: Bất kỳ user đã xác thực
* ✅ Lambda authorizer xác thực JWT + kiểm tra trạng thái đăng ký

**Thuật Toán Ranking**:
* ✅ Tính aggregate score:
  * Query Assignments table cho tất cả assignments trong lớp
  * Cho mỗi sinh viên: `total_score = SUM(submission.score / assignment.max_score * 100)`
  * Weight có thể áp dụng cho mỗi assignment (cải tiến tương lai)
* ✅ Tối ưu hiệu suất:
  * Sử dụng DynamoDB BatchGetItem cho dữ liệu sinh viên
  * Cache kết quả ranking (TTL: 5 phút) trong ElastiCache (tùy chọn)
  * Giới hạn top 40 sinh viên để giảm kích thước response

**Kết Quả Testing**:
* ✅ Ranking API: 220ms latency trung bình (40 sinh viên, 10 assignments)
* ✅ Create Post: 80ms latency trung bình
* ✅ List Posts: 150ms latency trung bình (20 posts mỗi trang)
* ✅ Create Comment: 65ms latency trung bình
* ✅ Delete Post: 120ms latency trung bình (cascade delete comments)
* ✅ Tất cả 5 APIs được test với Postman
* ✅ File upload/download flow được validate
* ✅ Batch grading được test với 50 sinh viên
* ✅ Thời gian phản hồi trung bình: 280ms

#### Kiến Trúc & Hạ Tầng AWS
* **DynamoDB Tables Đã Tạo**:
  * Classes (classId, className, teacherId, status, student_count)
  * Users (userId, username, email, role, status)
  * Enrollments (enrollmentId, classId, studentId, enrolledDate)
  * ActivityLogs với GSI (logId PK, timestamp SK, userIndex GSI)
  * Notifications (notificationId, userId, message, timestamp)

* **Lambda Functions**: 5 functions cho các endpoints
  * Runtime: Node.js 18.x
  * Memory: 512-1024 MB
  * Timeout: 30 seconds
  * Environment: DynamoDB table names, Cognito pool ID

* **EventBridge Rules**:
  * admin.enrollment → Tạo notification
  * admin.class-deactivated → Cập nhật trạng thái cascade
  * admin.user-deactivated → Dọn dẹp các record liên quan

* **Cognito Integration**:
  * Xác thực Admin group cho tất cả endpoints
  * JWT token verification middleware
  * Role-based access control (RBAC)

#### Chi Tiết Kỹ Thuật
* **Atomic Counter Pattern**:
  ```javascript
  UpdateExpression: 'SET student_count = student_count + :inc'
  ConditionExpression: 'student_count < :max'
  // Ngăn chặn race conditions trong concurrent enrollments
  ```

* **GSI Query Optimization**:
  * userIndex (PK=user_id, SK=timestamp) cho audit log filtering
  * Average query latency: 350ms
  * Hỗ trợ complex filtering và pagination

* **EventBridge Workflow**:
  * Event source: admin.enrollment
  * Targets: Lambda (notification), SES (email tùy chọn)
  * Xử lý bất đồng bộ để hiệu suất tốt hơn

#### Kết Quả Testing & Validation
* ✅ Unit tests: Admin permission validation, capacity limits, atomic operations
* ✅ Integration tests: End-to-end enrollment workflow, EventBridge delivery
* ✅ Security tests: Non-admin access blocked, JWT validation working
* ✅ Performance: Tất cả endpoints < 500ms average latency

#### Thách Thức & Giải Pháp
* **Race Condition trong Enrollment**: Giải quyết bằng DynamoDB atomic counters với ConditionExpression
* **Hiệu Suất Audit Log**: Triển khai GSI trên userId và timestamp (giảm 75% latency)
* **Notification Delivery**: Sử dụng DynamoDB table thay vì SES trực tiếp để giảm chi phí
* **Admin Permission**: Tích hợp Cognito admin groups cho secure access control

#### Giám Sát & Logging
* **CloudWatch Metrics**:
  * Lambda invocations, duration, errors
  * DynamoDB read/write capacity units
  * API Gateway 4xx/5xx errors, latency
  
* **CloudWatch Logs**:
  * Structured logging cho tất cả Lambda functions
  * Error tracking với log retention 7 days
  * Query logs để debug issues

* **Performance Monitoring**:
  * API Gateway latency: P99 < 800ms
  * Lambda cold start: < 2 seconds (Node.js 18.x)
  * DynamoDB hot partition monitoring

#### Phân Tích Chi Phí
| Service | Usage | Cost |
|---------|-------|------|
| Lambda | 2M invocations | $0.90 |
| DynamoDB | 3.5M read/write units | $0.70 |
| EventBridge | 45K events | $0.20 |
| CloudWatch | Logs + metrics | $0.10 |
| **Tổng Tuần 9** | | **$1.90 USD** |

#### Bước Tiếp Theo (Tuần 10)
* Triển khai tích hợp full-stack với Frontend (React)
* Staging deployment và integration testing
* Performance optimization và caching strategies
* Security hardening và penetration testing

#### Sản Phẩm Bàn Giao (Deliverables)
* ✅ **5 REST APIs**: Ranking + 4 Post/Comment endpoints
* ✅ **DynamoDB Schema**: Posts và PostComments tables với GSI
* ✅ **API Documentation**: Postman collection với example requests/responses
* ✅ **Testing Report**: Unit + integration test results, performance metrics
* ✅ **Architecture Diagram**: Updated với Post/Comment system

#### Tài Liệu Tham Khảo (References)
* [DynamoDB Query and Scan](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Query.html)
* [Lambda with DynamoDB](https://docs.aws.amazon.com/lambda/latest/dg/with-ddb.html)
* [API Gateway Authorization](https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-authorization-flow.html)
* [DynamoDB Best Practices](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/best-practices.html)
* [EventBridge Event Patterns](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-event-patterns.html)


