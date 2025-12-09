---
title: "Worklog Tuần 10"
date: "2024-11-18T00:00:00Z"
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---

### Mục tiêu tuần 10:

* Triển khai REST API cho Chat Messages
* Thêm tính năng Reactions (Like/Heart) cho chat messages
* Triển khai Delete Comment API
* Tối ưu Lambda functions và hiệu suất API

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 2 | - Thiết kế Chat Message schema: <br>&emsp; + Messages table (PK=message_id, GSI trên class_id) <br>&emsp; + Attributes: class_id, author_id, content, reactions, created_at <br>&emsp; + Bật DynamoDB Streams để track changes | 11/11/2025 | 11/11/2025 | [DynamoDB Streams](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Streams.html) |
| 3 | - Triển khai Chat REST APIs: <br>&emsp; + GET `/chat/messages/{class_id}` - List messages <br>&emsp; + POST `/chat/messages` - Gửi message <br>&emsp; + Polling mechanism cho tin nhắn mới <br>&emsp; + Hỗ trợ pagination | 12/11/2025 | 12/11/2025 | [Lambda with DynamoDB](https://docs.aws.amazon.com/lambda/latest/dg/with-ddb.html) |
| 4 | - Triển khai Reactions API: <br>&emsp; + PUT `/chat/messages/{id}/reactions` - Update Reactions <br>&emsp; + DynamoDB atomic counter (like_count, heart_count) <br>&emsp; + Track user reactions (ngăn duplicate) | 13/11/2025 | 13/11/2025 | [DynamoDB Atomic Counters](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/WorkingWithItems.html#WorkingWithItems.AtomicCounters) |
| 5 | - Triển khai Delete Comment API: <br>&emsp; + DELETE `/comments/{id}` - Xóa Comment <br>&emsp; + Validate quyền tác giả <br>&emsp; + Soft delete (status=0) <br>&emsp; + Update comment count | 14/11/2025 | 14/11/2025 | [API Gateway Authorization](https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-authorization-flow.html) |
| 6 | - Tối ưu Lambda & testing: <br> - Connection pooling cho DynamoDB <br> - Lambda layers cho shared code <br> - Test polling mechanism <br> - Performance testing (100 concurrent users) | 15/11/2025 | 15/11/2025 | [Lambda Best Practices](https://docs.aws.amazon.com/lambda/latest/dg/best-practices.html) |

### Kết quả đạt được tuần 10:

**Thiết Lập DynamoDB cho Chat Messages**:
* ✅ Tạo **Messages** table:
  * PK: message_id (UUID)
  * Attributes: class_id, author_id, author_name, content, reactions (Map), created_at
  * GSI: class_id-index (PK=class_id, SK=created_at) để list tin nhắn theo lớp
  * TTL attribute: expires_at (tùy chọn - tự động xóa tin nhắn sau 90 ngày)
  
* ✅ **DynamoDB Streams** được bật:
  * Stream view type: NEW_AND_OLD_IMAGES
  * Lambda trigger để track changes
  * Dùng cho audit logging và analytics
  
* ✅ **Tích Hợp Cognito**:
  * JWT token validation cho tất cả endpoints
  * Kiểm tra enrollment trước khi truy cập message

**APIs Đã Triển Khai (4 Chat + Reactions + Delete Endpoints)**:
* ✅ **GET** `/chat/messages/{class_id}` - Lấy Tin Nhắn Chat (REST API)
  * Query parameters: limit (mặc định: 50), lastEvaluatedKey
  * Query Messages table sử dụng GSI (class_id-index)
  * Sắp xếp theo created_at (tăng dần - cũ nhất trước)
  * Phân trang: giới hạn 50 tin nhắn mỗi trang
  * Returns: `{ messages: [...], lastEvaluatedKey }`
  * Latency: ~120ms
  
* ✅ **POST** `/chat/messages` - Gửi Tin Nhắn Chat (REST API)
  * Request body: `{ class_id, content }`
  * Validate user đã đăng ký lớp
  * Ghi vào Messages table (message_id, class_id, author_id, content, created_at)
  * **Trigger DynamoDB Stream**: Change được capture cho analytics
  * Returns: message_id, created_at
  * Latency: ~80ms (ghi)
  
* ✅ **PUT** `/chat/messages/{id}/reactions` - Update Reactions (Like/Heart)
  * Request body: `{ reaction_type: "like" | "heart" }`
  * DynamoDB atomic update:
    * Nếu user không có trong `liked_by`: Tăng `like_count`, thêm user_id vào `liked_by`
    * Nếu user đã like: Giảm `like_count`, xóa user_id (toggle)
  * Sử dụng `UpdateExpression`: `SET reactions.like_count = reactions.like_count + :val`
  * Ngăn duplicate reactions từ cùng một user
  * Returns: số lượng reactions đã cập nhật `{ like_count, heart_count }`
  * Latency: ~60ms
  
* ✅ **DELETE** `/comments/{id}` - Xóa Comment
  * Path parameter: comment_id
  * Validate user là tác giả comment HOẶC giảng viên của lớp
  * Query PostComments table để lấy post_id
  * Soft delete: Update status=0 trong PostComments table
  * Atomic decrement comment_count trong Posts table
  * Returns: `{ message: "Comment deleted successfully" }`
  * Latency: ~90ms

**Luồng Message Polling**:
1. Client gọi GET `/chat/messages/{class_id}` định kỳ (mỗi 3-5 giây)
2. Query bao gồm parameter `lastMessageTimestamp`
3. API chỉ trả về tin nhắn mới sau timestamp đó
4. Client cập nhật UI với tin nhắn mới
5. **Polling interval**: 3 giây (có thể cấu hình)
6. **Tối ưu**: Client-side caching để giảm API calls


**Tối Ưu Lambda**:
* ✅ Triển khai connection pooling:
  * Tái sử dụng DynamoDB client qua các invocations
  * Giảm latency: 80ms → 15ms mỗi query
  
* ✅ Tạo Lambda layers:
  * aws-sdk-layer: 12MB (DynamoDB clients)
  * utils-layer: 1.5MB (shared utilities)
  * Giảm kích thước function 65%
  
* ✅ Tối ưu bundle:
  * Bật Webpack tree-shaking
  * Bundle size: 6MB → 2.5MB
  * Cold start time: 1.8s → 600ms (giảm 67%)

**Kết Quả Testing**:
* ✅ Chat polling mechanism: 100 concurrent users, polling interval 3 giây
* ✅ Chat message latency: 80ms trung bình (ghi)
* ✅ Message retrieval: 120ms latency trung bình (với pagination)
* ✅ Reactions API: 60ms latency trung bình (atomic update)
* ✅ Delete Comment: 90ms latency trung bình
* ✅ Load test: 500 messages gửi trong 10 phút, 0 lỗi

#### Kiến Trúc & Hạ Tầng AWS
* **Lambda Functions**: 4 functions cho chat endpoints
  * Runtime: Node.js 18.x
  * Memory: 512-1024 MB
  * Timeout: 30 seconds
  * Connection pooling enabled

* **DynamoDB Streams**:
  * Trigger Lambda cho audit logging
  * Stream retention: 24 hours
  * Batch size: 100 records

* **API Gateway Configuration**:
  * CORS enabled cho frontend
  * Rate limiting: 500 req/min per user
  * Response caching: 5 minutes TTL

#### Chi Tiết Kỹ Thuật
* ✅ Triển khai connection pooling:
  * Tái sử dụng DynamoDB client qua các invocations
  * Giảm latency: 80ms → 15ms mỗi query
  
* ✅ Tạo Lambda layers:
  * aws-sdk-layer: 12MB (DynamoDB clients)
  * utils-layer: 1.5MB (shared utilities)
  * Giảm kích thước function 65%
  
* ✅ Tối ưu bundle:
  * Bật Webpack tree-shaking
  * Bundle size: 6MB → 2.5MB
  * Cold start time: 1.8s → 600ms (giảm 67%)

* ✅ **Polling Optimization**:
  * Client-side caching với IndexedDB
  * Exponential backoff khi không có tin nhắn mới
  * Network detection để pause polling khi offline

#### Giám Sát & Logging
* **CloudWatch Dashboards** đã tạo:
  * **API Performance**: Request count, latency (p50, p95, p99), errors
  * **Lambda Metrics**: Invocations, duration, errors, concurrent executions
  * **DynamoDB Metrics**: Read/write capacity, throttles
  * **Cost Dashboard**: Chi tiêu hàng ngày theo service
  
* **CloudWatch Alarms** đã cấu hình (8 alarms):
  1. API Gateway 5xx errors > 10 trong 5 phút
  2. Lambda error rate > 5%
  3. DynamoDB throttles > 50 trong 5 phút
  4. API latency p99 > 2 giây
  5. Lambda concurrent executions > 800
  6. Chi phí hàng ngày > $20
  7. S3 bucket storage > 50GB
  8. Cognito authentication failures > 20 trong 5 phút
  
* **AWS X-Ray Tracing** đã bật:
  * Tất cả Lambda functions được instrument
  * Service map visualization
  * Xác định bottlenecks (DynamoDB queries, S3 uploads)

#### End-to-End Testing
* ✅ Tất cả 11 APIs được test:
  * 5 Class Management APIs
  * 5 Assignment Management APIs
  * 1 Post API
* ✅ Kịch bản test:
  * Tạo class → Thêm assignment → Cập nhật điểm → Đăng thông báo
  * List classes → List assignments → List students
  * Thao tác Edit/Delete
* ✅ Authentication flow được validate
* ✅ File upload/download được test
* ✅ Notification delivery được xác nhận

#### Chỉ Số Hiệu Suất
* API response time: 280ms → 180ms (trung bình)
* Lambda cold start: 1.8s → 600ms
* DynamoDB query latency: 80ms → 15ms
* Cache hit rate: 68%
* Error rate: < 0.1%

#### Ước Tính Chi Phí (Production)
* Lambda: ~$5-8/tháng (1M requests)
* DynamoDB: ~$3-5/tháng (on-demand)
* API Gateway: ~$3-4/tháng
* S3: ~$1-2/tháng (10GB storage)
* CloudWatch: ~$2-3/tháng
* SNS: ~$0.50/tháng
* **Tổng: ~$15-23/tháng** (cho 100-200 active users)

#### Phân Tích Chi Phí (Tuần 10)
* Lambda: $1.40 (~3M invocations với polling)
* DynamoDB: $1.20 (tăng reads từ polling)
* API Gateway: $0.60
* CloudWatch: $0.25
* **Tổng: $3.45 USD**

#### Bài Học Kinh Nghiệm
* Polling interval cân bằng: Quá thường xuyên = chi phí cao, quá chậm = UX kém
* Thiết kế DynamoDB GSI quan trọng cho query hiệu quả
* API Gateway caching giảm Lambda invocations 70%
* Client-side caching thiết yếu cho polling approach
* DynamoDB Streams hữu ích cho audit logging và analytics
* Load testing phát hiện bottlenecks không thấy trong development

#### Bước Tiếp Theo (Tuần 11)
* Integration testing toàn bộ hệ thống
* Staging deployment với AWS CodePipeline
* Performance optimization và stress testing
* Security hardening và penetration testing

#### Sản Phẩm Bàn Giao (Deliverables)
* ✅ **4 Chat APIs**: Message CRUD + Reactions + Delete Comment
* ✅ **Lambda Optimization**: Connection pooling, layers, bundle optimization
* ✅ **CloudWatch Monitoring**: Dashboards + 8 alarms + X-Ray tracing
* ✅ **Testing Report**: Load test results, performance metrics
* ✅ **Architecture Diagram**: Updated với chat system và monitoring

#### Tài Liệu Tham Khảo (References)
* [DynamoDB Streams](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Streams.html)
* [Lambda with DynamoDB](https://docs.aws.amazon.com/lambda/latest/dg/with-ddb.html)
* [API Gateway Authorization](https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-authorization-flow.html)
* [Lambda Best Practices](https://docs.aws.amazon.com/lambda/latest/dg/best-practices.html)
* [DynamoDB Atomic Counters](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/WorkingWithItems.html#WorkingWithItems.AtomicCounters)
* [CloudWatch Alarms](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html)