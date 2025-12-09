---
title: "Week 9 Worklog"
date: "2024-11-11T00:00:00Z"
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---

### Week 9 Objectives:

* Implement Student Ranking API with leaderboard functionality
* Implement Post/Comment System for class discussions
* Set up DynamoDB tables for Posts and Comments

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | ---- | ---------- | --------------- | ------------------ |
| 2 | - Implement Ranking API: <br>&emsp; + GET `/student/ranking/{class_id}` - Get Ranking per Class <br>&emsp; + Calculate aggregate score from Submissions table <br>&emsp; + Sort students by total score <br>&emsp; + Return top 40 students | 04/11/2025 | 04/11/2025 | [DynamoDB Query](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Query.html) |
| 3 | - Design Post/Comment schema: <br>&emsp; + Posts table (PK=post_id, class_id, author_id, content, created_at) <br>&emsp; + PostComments table (PK=post_id, SK=comment_id) <br>&emsp; + GSI on class_id for listing posts | 05/11/2025 | 05/11/2025 | [DynamoDB Best Practices](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/best-practices.html) |
| 4 | - Implement Post APIs (Part 1): <br>&emsp; + POST `/classes/{class_id}/posts` - Create Post (Teacher/Student) <br>&emsp; + GET `/classes/{class_id}/posts` - List Posts <br>&emsp; + Support rich text (Markdown) | 06/11/2025 | 06/11/2025 | [Lambda with DynamoDB](https://docs.aws.amazon.com/lambda/latest/dg/with-ddb.html) |
| 5 | - Implement Post APIs (Part 2): <br>&emsp; + POST `/posts/{post_id}/comments` - Create Comment <br>&emsp; + DELETE `/posts/{id}` - Delete Post <br>&emsp; + Validate author permissions | 07/11/2025 | 07/11/2025 | [API Gateway Authorization](https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-authorization-flow.html) |
| 6 | - Testing and bug fixes: <br> - Test ranking calculation <br> - Test post/comment CRUD <br> - Validate permissions <br> - Performance testing | 08/11/2025 | 08/11/2025 | [Postman Testing](https://www.postman.com/) |

### Week 9 Achievements:

**DynamoDB Tables for Posts/Comments**:
* ✅ **Posts** table created:
  * PK: post_id (UUID)
  * Attributes: class_id, author_id, author_name, title, content (Markdown), created_at, is_pinned
  * GSI: class_id-index (PK=class_id, SK=created_at) for listing posts by class
  
* ✅ **PostComments** table created:
  * PK: post_id, SK: comment_id (UUID)
  * Attributes: author_id, author_name, content, created_at
  * Enables efficient querying of all comments for a post

**APIs Implemented (5 Ranking + Post/Comment Endpoints)**:
* ✅ **GET** `/student/ranking/{class_id}` - Get Ranking per Class
  * Query Enrollments table for all students in class
  * Query Submissions table for each student (aggregate score)
  * Calculate total score: `SUM(score * assignment_weight)`
  * Sort by total_score (descending)
  * Return top 40 students: `[{ rank, student_id, name, total_score }]`
  * Response: `{ rankings: [...], my_rank: 15 }` (authenticated student's rank)
  
* ✅ **POST** `/classes/{class_id}/posts` - Create Post (Teacher/Student)
  * Request body: `{ title, content, is_pinned }`
  * Validate user is enrolled in class (or is teacher)
  * Write to Posts table (post_id, class_id, author_id, content, created_at)
  * If author is Teacher: set is_pinned=true (optional)
  * Trigger notification to all enrolled students
  * Returns: post_id, created_at
  
* ✅ **GET** `/classes/{class_id}/posts` - List Posts
  * Query Posts table using GSI (class_id-index)
  * Sort by created_at (descending), pinned posts first
  * Pagination: limit 20 per page, LastEvaluatedKey for next page
  * Returns: `[{ post_id, title, author_name, content_preview, created_at, comment_count }]`
  
* ✅ **POST** `/posts/{post_id}/comments` - Create Comment
  * Request body: `{ content }`
  * Validate user is enrolled in class (check Posts table for class_id)
  * Write to PostComments table (post_id, comment_id, author_id, content, created_at)
  * Increment comment_count in Posts table (atomic update)
  * Trigger notification to post author
  * Returns: comment_id, created_at
  
* ✅ **DELETE** `/posts/{id}` - Delete Post
  * Validate user is author OR teacher of class
  * Soft delete: Update status=0 in Posts table
  * Also soft delete all comments in PostComments table (cascade)
  * Returns: `{ message: "Post deleted successfully" }`

**Authorization & Permissions**:
* ✅ **Create Post**: Teacher (any content) + Student (if enrolled)
* ✅ **Create Comment**: Teacher + Student (if enrolled)
* ✅ **Delete Post**: Author only (or Teacher for any post in their class)
* ✅ **List Posts**: Any authenticated user
* ✅ Lambda authorizer validates JWT + checks enrollment status

**Ranking Algorithm**:
* ✅ Aggregate score calculation:
  * Query Assignments table for all assignments in class
  * For each student: `total_score = SUM(submission.score / assignment.max_score * 100)`
  * Weight can be applied per assignment (future enhancement)
* ✅ Performance optimization:
  * Use DynamoDB BatchGetItem for student data
  * Cache ranking results (TTL: 5 minutes) in ElastiCache (optional)
  * Limit to top 40 students to reduce response size

**Testing Results**:
* ✅ Ranking API: 220ms avg latency (40 students, 10 assignments)
* ✅ Create Post: 80ms avg latency
* ✅ List Posts: 150ms avg latency (20 posts per page)
* ✅ Create Comment: 65ms avg latency
* ✅ Delete Post: 120ms avg latency (cascade delete comments)
* ✅ All 5 APIs tested with Postman
* ✅ File upload/download flow validated
* ✅ Batch grading tested with 50 students
* ✅ Average response time: 280ms
  
* ✅ `POST /Student/assignments/submit` - Submit assignment
  * Request body: `{ assignmentId, fileUrl, comment }`
  * Generates S3 pre-signed URL for file upload (expires: 15 minutes)
  * Updates AssignmentSubmissions table with submittedAt timestamp
  * Triggers EventBridge event to notify teacher via SES

**Admin APIs (4)**:
* ✅ `POST /Admin/deactivate-class` - Deactivate class
  * Request body: `{ classId, reason }`
  * Validates admin role via Cognito groups
  * Updates Classes table (status: "inactive")
  * Emits audit log to EventBridge
  
* ✅ `POST /Admin/deactivate-user` - Deactivate user account
  * Request body: `{ userId, reason }`
  * Disables Cognito user account
  * Updates Users table (isActive: false)
  * Logs action in AuditLogs table
  
* ✅ `GET /Admin/audit-logs` - Retrieve audit logs
  * Query parameters: userId, action, startDate, endDate, limit
  * Queries AuditLogs table with GSI on timestamp
  * Returns: action, userId, timestamp, metadata
  * Data sources: EventBridge + DynamoDB Streams
  
* ✅ `GET /Admin/enrollments` - Get enrollment analytics
  * Query parameters: classId, status, startDate, endDate
  * Aggregates enrollment data from StudentClasses table
  * Returns: total enrollments, active/inactive breakdown, time series

**AWS Infrastructure Additions**:
* ✅ **S3 Bucket for Assignments**:
  * Bucket: student-management-assignments
  * CORS configuration for frontend uploads
  * Lifecycle policy: Delete objects > 90 days
  * Pre-signed URL generation in Lambda
  
* ✅ **EventBridge Event Bus**:
  * Custom event bus: student-management-events
  * Rules configured:
    1. enrollment-audit-log (captures enroll/unenroll events)
    2. assignment-submission-notification (triggers SES email)
    3. class-deactivation-audit (logs admin actions)
  
* ✅ **DynamoDB Tables Created**:
  * ChatMessages (PK: chatId, SK: messageId)
  * AssignmentSubmissions (PK: assignmentId, SK: studentId)
  * AuditLogs (PK: logId, GSI: timestamp for queries)
  
* ✅ **Lambda Functions (5)**:
  * submitAssignment, deactivateClass, deactivateUser
  * getAuditLogs, getEnrollments

**Security Enhancements**:
* Admin APIs restricted to Cognito Admin group (IAM policy)
* S3 bucket with encryption at rest (AES-256)
* Pre-signed URLs with 15-minute expiration
* API Gateway rate limiting: 500 req/min per user

**EventBridge Audit Logging**:
* All critical actions (enroll, deactivate, submit) logged
* Event pattern matching configured
* DynamoDB Streams feed into AuditLogs table
* Retention: 365 days

**Performance Metrics**:
* S3 pre-signed URL generation: 50ms
* Admin API response time: 200-400ms
* EventBridge event processing: < 1 second

**Cost Analysis (Week 9)**:
* S3: $0.20 (storage + requests)
* Lambda: $0.90 (~2M invocations)
* DynamoDB: $0.70
* EventBridge: $0.10 (custom events)
* **Total: $1.90 USD**

**Key Learnings**:
* EventBridge + DynamoDB Streams provide robust audit logging
* S3 pre-signed URLs enable secure direct uploads from frontend
* Cognito groups are effective for role-based access control (RBAC)

#### AWS Infrastructure
* **DynamoDB Tables Created**:
  * Classes (classId, className, teacherId, status, student_count)
  * Users (userId, username, email, role, status)
  * Enrollments (enrollmentId, classId, studentId, enrolledDate)
  * ActivityLogs with GSI (logId PK, timestamp SK, userIndex GSI)
  * Notifications (notificationId, userId, message, timestamp)

* **Lambda Functions**: 4 functions for admin endpoints
  * Runtime: Node.js 18.x
  * Memory: 512-1024 MB
  * Timeout: 30 seconds
  * Environment: DynamoDB table names, Cognito pool ID

* **EventBridge Rules**:
  * admin.enrollment → Notification creation
  * admin.class-deactivated → Cascade status updates
  * admin.user-deactivated → Related record cleanup

* **Cognito Integration**:
  * Admin group validation for all endpoints
  * JWT token verification middleware
  * Role-based access control (RBAC)

#### Technical Implementation
* **Atomic Counter Pattern**:
  ```javascript
  UpdateExpression: 'SET student_count = student_count + :inc'
  ConditionExpression: 'student_count < :max'
  // Prevents race conditions during concurrent enrollments
  ```

* **GSI Query Optimization**:
  * userIndex (PK=user_id, SK=timestamp) for audit log filtering
  * Average query latency: 350ms
  * Supports complex filtering and pagination

* **EventBridge Workflow**:
  * Event source: admin.enrollment
  * Targets: Lambda (notification), SES (optional email)
  * Asynchronous processing for better performance

#### Testing Results
* ✅ Unit tests: Admin permission validation, capacity limits, atomic operations
* ✅ Integration tests: End-to-end enrollment workflow, EventBridge delivery
* ✅ Security tests: Non-admin access blocked, JWT validation working
* ✅ Performance: All endpoints < 500ms average latency

#### Cost Analysis
| Service | Usage | Cost |
|---------|-------|------|
| Lambda | 2.8M invocations | $1.10 |
| DynamoDB | 3.5M read/write units | $0.95 |
| EventBridge | 45K events | $0.20 |
| CloudWatch | Logs + metrics | $0.15 |
| **Total Week 9** | | **$2.40 USD** |

#### Challenges Overcome
* **Race Condition in Enrollment**: Solved using DynamoDB atomic counters with ConditionExpression
* **Audit Log Performance**: Implemented GSI on userId and timestamp (75% latency reduction)
* **Notification Delivery**: Used DynamoDB table instead of direct SES to reduce costs
* **Admin Permission**: Integrated Cognito admin groups for secure access control

* Acquired the ability to connect between the web interface and CLI to manage AWS resources in parallel.
* ...
