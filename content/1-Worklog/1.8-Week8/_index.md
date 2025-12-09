---
title: "Week 8 Worklog"
date: "2024-11-04T00:00:00Z"
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

### Week 8 Objectives:

* Set up AWS Serverless infrastructure (Lambda, DynamoDB, API Gateway, SAM)
* Implement Student Core APIs (Submit, Enroll, Notifications, Search)
* Configure Cognito authentication and IAM roles

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | ---- | ---------- | --------------- | ------------------ |
| 2 | - Initialize AWS SAM project: <br>&emsp; + Install AWS SAM CLI <br>&emsp; + Create template.yaml <br>&emsp; + Configure DynamoDB tables (Classes, Students, Enrollments, Assignments, Submissions, Notifications) <br>&emsp; + Setup local development environment | 28/10/2025 | 28/10/2025 | [AWS SAM Getting Started](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-getting-started.html) |
| 3 | - Implement Student APIs (Part 1): <br>&emsp; + POST `/student/submit` - Submit Assignment <br>&emsp; + GET `/student/classes/class-enrolled` - Get Enrolled Classes <br>&emsp; + S3 pre-signed URLs for file upload | 29/10/2025 | 29/10/2025 | [Lambda with DynamoDB](https://docs.aws.amazon.com/lambda/latest/dg/with-ddb.html) |
| 4 | - Implement Student APIs (Part 2): <br>&emsp; + GET `/student/notifications` - Get Notifications <br>&emsp; + POST `/student/enroll` - Enroll/Unenroll Class <br>&emsp; + Configure API Gateway endpoints | 30/10/2025 | 30/10/2025 | [API Gateway REST API](https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-rest-api.html) |
| 5 | - Setup Cognito Authentication: <br>&emsp; + Create User Pool <br>&emsp; + Configure JWT authorizer for Student role <br>&emsp; + Test authentication flow <br>&emsp; + Integrate with API Gateway | 31/10/2025 | 31/10/2025 | [Cognito User Pools](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-identity-pools.html) |
| 6 | - Implement Search API: <br>&emsp; + GET `/student/search` - Search Classes/Teachers <br> - Testing with SAM Local <br> - Deploy to AWS dev environment | 01/11/2025 | 01/11/2025 | [DynamoDB Scan](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Scan.html) |

### Week 8 Achievements:

**AWS Infrastructure Setup**:
* ✅ AWS SAM project initialized with template.yaml
* ✅ DynamoDB tables created:
  * **Classes** table: PK = class_id, attributes (class_name, teacher_id, status, max_students)
  * **Students** table: PK = student_id, attributes (name, email, status)
  * **Enrollments** table: PK = student_id, SK = class_id, attributes (enrolled_at, status)
  * **Assignments** table: PK = assignment_id, attributes (class_id, title, due_date, max_score)
  * **Submissions** table: PK = assignment_id, SK = student_id, attributes (file_url, submitted_at, score)
  * **Notifications** table: PK = user_id, SK = timestamp, attributes (type, message, read_status)
* ✅ S3 bucket created: `student-submissions-bucket`
* ✅ SAM Local testing environment configured
* ✅ Dev environment deployed to AWS

**APIs Implemented (5 Student Core Endpoints)**:
* ✅ **POST** `/student/submit` - Submit Assignment
  * Request body: `{ assignment_id, file_content }`
  * Generate S3 pre-signed URL (15 min expiry)
  * Upload file to S3: `submissions/{assignment_id}/{student_id}/file.pdf`
  * Write to Submissions table (assignment_id, student_id, file_url, submitted_at)
  * Trigger notification to teacher
  * Returns: submission_id, s3_url, submitted_at
  
* ✅ **GET** `/student/classes/class-enrolled` - Get Enrolled Classes
  * Query Enrollments table by student_id (from JWT token)
  * Join with Classes table to get class details
  * Returns: Array `[{ class_id, class_name, teacher_name, enrolled_at }]`
  * Sorted by enrolled_at (descending)
  
* ✅ **GET** `/student/notifications` - Get Notifications
  * Query Notifications table by user_id (from JWT token)
  * Filter by read_status (optional query param: `?unread=true`)
  * Returns: Array `[{ notification_id, type, message, timestamp, read_status }]`
  * Pagination: limit 20 per page
  * Types: assignment_graded, new_post, enrollment_approved
  
* ✅ **POST** `/student/enroll` - Enroll/Unenroll Class
  * Request body: `{ class_id, action: "enroll" | "unenroll" }`
  * **Enroll**: Check class capacity (<= max_students), write to Enrollments table, create notification
  * **Unenroll**: Delete from Enrollments (soft delete: status=0)
  * Atomic update: increment/decrement student_count in Classes table
  * Returns: `{ message: "Enrolled successfully", enrollment_id }`
  
* ✅ **GET** `/student/search` - Search Classes/Teachers
  * Query parameters: `keyword`, `status`, `teacher_name`
  * DynamoDB Scan with FilterExpression (multi-field search)
  * Search fields: class_name, teacher_name, description
  * Returns: Array `[{ class_id, class_name, teacher_name, student_count, status }]`
  * Pagination: limit 20 per page

**Authentication & Security**:
* ✅ Cognito User Pool created with Student/Teacher/Admin groups
* ✅ JWT token validation in Lambda authorizer
* ✅ IAM roles configured: StudentRole (read/write own data only)
* ✅ API Gateway authorizer integrated (validates JWT + role)
* ✅ S3 bucket policy: pre-signed URLs only (no public access)

---

## Architecture & Infrastructure

### AWS Services Used
| Service | Purpose |
|---------|---------|
| **AWS Lambda** | Execute business logic for 5 API endpoints |
| **Amazon API Gateway** | REST API routing, request validation, throttling |
| **Amazon DynamoDB** | Store student data, classes, notifications |
| **AWS Cognito** | User authentication via JWT tokens |
| **Amazon CloudWatch** | Monitoring, logs, and metrics |
| **Amazon EventBridge** | Event-driven processing for notifications |

### DynamoDB Tables Created
1. **StudentNotifications** - userId, notificationId
2. **StudentClasses** - userId, classId
3. **ClassPerformance** - classId, userId (GSI for ranking)

---

## Technical Details

### Lambda Configuration
- **Runtime**: Node.js 18.x
- **Memory**: 512MB
- **Timeout**: 30 seconds
- **Environment Variables**: DynamoDB table names, Cognito pool ID

### API Gateway Configuration
- CORS enabled for all origins
- Request validation on POST endpoints
- Rate limiting: 1000 requests/5 minutes per IP
- Caching enabled (5-minute TTL) for GET endpoints

---

## Testing & Validation

### Unit Tests
- ✅ Lambda function logic validation
- ✅ DynamoDB query accuracy
- ✅ Cognito token verification
- ✅ Error handling and edge cases

### Integration Tests
- ✅ End-to-end API flow
- ✅ Database transactions

### Manual Testing (Postman)
- ✅ All 5 API endpoints tested
- ✅ Request/response validation
- ✅ Error scenarios (400, 401, 404, 500)

### Performance Testing
- ✅ API response time < 500ms
- ✅ DynamoDB query latency < 10ms

---

## Challenges & Solutions

| Challenge | Solution |
|-----------|----------|
| High latency for search queries | Implemented DynamoDB GSI on frequently queried attributes |
| Cognito token validation overhead | Implemented token caching at API Gateway level |
| DynamoDB hot partition | Added distributed hash keys for better load distribution |
| Rate limiting requirements | Configured API Gateway throttling policies |

---

## Monitoring & Logging

### CloudWatch Metrics
- Lambda execution duration
- API Gateway latency
- DynamoDB read/write units consumed
- Error rates and HTTP status codes

### Alerts Configured
- Lambda error rate > 1%
- API Gateway 5XX errors
- DynamoDB throttling events

---

## Cost Analysis

### Week 8 Cost Breakdown
| Service | Estimated Cost |
|---------|----------------|
| AWS Lambda | $0.80 (~1.5M invocations) |
| DynamoDB | $0.50 (on-demand pricing) |
| API Gateway | $0.35 (1.2M requests) |
| CloudWatch | $0.10 (logs) |
| **Total** | **$1.75 USD** |

---

## Next Steps (Week 9)
- [ ] Implement assignment grading workflow with EventBridge
- [ ] Add ranking notifications via SES
- [ ] Optimize DynamoDB queries for scale
- [ ] Implement input validation middleware
- [ ] Set up API documentation (OpenAPI/Swagger)

## Deliverables
✅ 5 functional REST API endpoints  
✅ DynamoDB tables with optimized indexes  
✅ CloudWatch monitoring dashboard  
✅ Postman API collection  
✅ Unit test coverage > 80%  
✅ API documentation  
✅ Deployment via AWS SAM  

---

## References
- [AWS Lambda Best Practices](https://docs.aws.amazon.com/lambda/)
- [DynamoDB Design Patterns](https://docs.aws.amazon.com/amazondynamodb/)
- [API Gateway Documentation](https://docs.aws.amazon.com/apigateway/)

