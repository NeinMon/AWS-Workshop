---
title: "Week 11 Worklog"
date: "2024-11-25T00:00:00Z"
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---

### Week 11 Objectives:

* Implement Admin Management APIs (Deactivate Class, Deactivate User, Audit Logs)
* Conduct integration testing for all 14 APIs (Student + Admin)
* Create API documentation with Swagger/OpenAPI
* Prepare staging environment

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | ---- | ---------- | --------------- | ------------------ |
| 2 | - Implement Admin API (Part 1): <br>&emsp; + PATCH `/admin/classes/{id}` - Deactivate Class <br>&emsp; + Validate admin role <br>&emsp; + Soft delete (status=0) <br>&emsp; + Trigger audit log | 18/11/2025 | 18/11/2025 | [API Gateway Authorization](https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-authorization-flow.html) |
| 3 | - Implement Admin API (Part 2): <br>&emsp; + PATCH `/admin/users/{id}` - Deactivate User <br>&emsp; + Update status in Users table <br>&emsp; + Disable Cognito user <br>&emsp; + Trigger audit log | 19/11/2025 | 19/11/2025 | [Cognito Admin APIs](https://docs.aws.amazon.com/cognito/latest/developerguide/user-pool-settings-admin-apis.html) |
| 4 | - Implement Admin API (Part 3): <br>&emsp; + GET `/admin/audit-logs` - Get Audit Logs <br>&emsp; + Query ActivityLogs table <br>&emsp; + Filter by user_id, class_id, timestamp <br>&emsp; + Pagination support | 20/11/2025 | 20/11/2025 | [DynamoDB GSI Queries](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/GSI.html) |
| 5 | - Integration testing: <br>&emsp; + Test all 14 APIs (10 Student + 3 Admin + 1 Delete Comment) <br>&emsp; + Test admin authorization <br>&emsp; + Test audit logging <br>&emsp; + Load testing with 100 concurrent users | 21/11/2025 | 21/11/2025 | [Postman Collections](https://learning.postman.com/docs/collections/collections-overview/) |
| 6 | - API Documentation & Staging: <br> - Generate OpenAPI 3.0 specification <br> - Deploy Swagger UI <br> - Deploy to staging environment <br> - Validate all services | 22/11/2025 | 22/11/2025 | [OpenAPI Specification](https://swagger.io/specification/) |

### Week 11 Achievements:

**DynamoDB Table for Audit Logs**:
* ✅ **ActivityLogs** table created:
  * PK: log_id (UUID)
  * Attributes: user_id, class_id, action_type, details (JSON), timestamp
  * GSI: user_id-index (PK=user_id, SK=timestamp) for filtering by user
  * GSI: class_id-index (PK=class_id, SK=timestamp) for filtering by class
  * TTL: expires_at (auto-delete logs after 365 days)

**APIs Implemented (3 Admin Management Endpoints)**:
* ✅ **PATCH** `/admin/classes/{id}` - Deactivate Class
  * Path parameter: class_id
  * Validate user has Admin role (from JWT token)
  * Update Classes table: SET status = 0 (soft delete)
  * Keep historical data (enrollments, assignments, submissions)
  * Trigger audit log: `{ action_type: "class_deactivated", class_id, admin_id, timestamp }`
  * Returns: `{ message: "Deleted" }`
  * Latency: ~80ms
  
* ✅ **PATCH** `/admin/users/{id}` - Deactivate User
  * Path parameter: user_id
  * Validate user has Admin role
  * **Step 1**: Update Users table: SET status = 0
  * **Step 2**: Disable user in Cognito User Pool using AdminDisableUser API
  * User cannot login after deactivation
  * Trigger audit log: `{ action_type: "user_deactivated", user_id, admin_id, timestamp }`
  * Returns: `{ message: "Deleted" }`
  * Latency: ~120ms (DynamoDB + Cognito)
  
* ✅ **GET** `/admin/audit-logs` - Get Audit Logs
  * Query parameters:
    - `user_id` (optional): Filter by user
    - `class_id` (optional): Filter by class
    - `timestamp` (optional): Filter by date range
    - `limit` (default: 100): Number of results
  * Query ActivityLogs table:
    - If user_id: Use user_id-index GSI
    - If class_id: Use class_id-index GSI
    - Else: Scan table (limited to last 7 days)
  * Returns: `{ results: [{ log_id, user_id, class_id, action_type, details, timestamp }] }`
  * Pagination: LastEvaluatedKey for next page
  * Latency: ~200ms (GSI query), ~1.5s (full scan)

**Authorization & Security**:
* ✅ **Admin Role Validation**:
  * Lambda authorizer checks JWT token for `cognito:groups` claim
  * Only users in "Admin" group can access `/admin/*` endpoints
  * Returns 403 Forbidden if not admin
  
* ✅ **Audit Logging Implemented**:
  * All admin actions logged to ActivityLogs table
  * Action types: class_deactivated, user_deactivated, class_created, enrollment_added
  * Details stored as JSON: `{ old_value, new_value, reason }`
  * Enables compliance and security tracking

**Integration Testing**:
* ✅ **Test Suite Created**: 35+ integration tests
  * Student APIs: 10 tests (submit, enroll, notifications, search, ranking)
  * Admin APIs: 6 tests (deactivate class, deactivate user, audit logs)
  * Post/Comment APIs: 8 tests (create post, list posts, create comment, delete post, delete comment)
  * Chat APIs: 4 tests (send message, get messages, reactions, delete comment)
  * Authentication: 6 tests (JWT validation, role-based access)
  
* ✅ **Test Coverage**: All 14 APIs validated
  * Create, Read, Update, Delete operations
  * Error handling (400, 401, 403, 404, 500)
  * Edge cases (empty data, invalid input, concurrent requests)
  * Authorization checks (Student cannot access Admin APIs)
  
* ✅ **Load Testing Results**:
  * Tool: Apache JMeter
  * Configuration: 100 concurrent users, 10 minutes
  * Results:
    - Average response time: 220ms
    - 95th percentile: 580ms
    - Error rate: 0.08%
    - Throughput: 450 requests/second
    - Admin APIs: 180ms avg (audit log query: 250ms)

**API Documentation**:
* ✅ **OpenAPI 3.0 Specification**:
  * 14 endpoints fully documented (10 Student + 3 Admin + 1 Delete Comment)
  * Request/response schemas
  * Authentication requirements (JWT + role-based)
  * Error codes and messages
  * Example requests/responses
  
* ✅ **Swagger UI Deployed**:
  * URL: https://api-docs.student-mgmt.com
  * Interactive API testing
  * Code generation (JavaScript, Python, cURL)
  * Separate sections: Student APIs, Admin APIs, Chat APIs
  
* ✅ **Integration Guide Created**:
  * Authentication flow (Cognito JWT)
  * Role-based access control (Student, Teacher, Admin)
  * API endpoint usage
  * File upload/download procedures
  * Rate limiting and best practices

**Staging Deployment**:
* ✅ Staging environment deployed:
  * API Gateway: https://staging-api.student-mgmt.com
  * All Lambda functions deployed (17 functions)
  * DynamoDB tables with test data (9 tables)
  * S3 buckets configured
  * Cognito user pool with test users (10 students, 3 teachers, 2 admins)
  * REST APIs for chat with polling mechanism
  
* ✅ Staging validation:
  * All 14 APIs functional
  * Chat polling mechanism working
  * Audit logging verified
  * Admin deactivation tested (class + user)
  * Performance: 85% code coverage
  * Lambda cold start: <1.5s (warm: <200ms)
  * DynamoDB read/write latency: <50ms
  * S3 upload/download speed: >10MB/s
  * Email notification delivery: <30s

**Tools & Technologies Used**:
* **API Development**: AWS Lambda, API Gateway
* **Database**: Amazon DynamoDB with 9 tables
* **Authentication**: Amazon Cognito User Pools with JWT
* **Storage**: Amazon S3 for file storage
* **Monitoring**: CloudWatch Logs, CloudWatch Metrics, X-Ray tracing
* **Testing**: Postman/Newman, JMeter (load testing), Jest (unit tests)
* **Documentation**: Swagger/OpenAPI 3.0, API Blueprint
* **CI/CD**: AWS CodePipeline, CodeBuild, CodeDeploy
* **IaC**: AWS CDK (TypeScript) or CloudFormation
* **Development**: VS Code, AWS Toolkit, CloudShell

**Proposal Document Finalized**:
* ✅ **Architecture Diagrams**:
  * High-level system architecture
  * AWS services diagram
  * API flow diagrams
  * Database schema (DynamoDB tables)
  * Authentication flow
  
* ✅ **Cost Analysis**:
  * Development cost: $0 (Free Tier)
  * Production estimate: $7-20/month (100-500 users)
  * Breakdown by service
  * Scaling projections
  
* ✅ **Risk Assessment**:
  * 8 identified risks with mitigation strategies:
    1. Lambda cold starts → Provisioned concurrency
    2. DynamoDB throttling → On-demand capacity
    3. Cost overruns → Budget alerts
    4. Security vulnerabilities → WAF, encryption
    5. Data loss → Automated backups
    6. API downtime → Multi-AZ deployment
    7. Slow performance → Caching, optimization
    8. Scalability limits → Auto-scaling configured

**Demo Preparation**:
* ✅ Demo scenarios created:
  * Scenario 1: Create class and add students
  * Scenario 2: Create assignment and update grades
  * Scenario 3: Post announcement to class
  * Scenario 4: List and filter data
  
* ✅ Test data prepared:
  * 5 test classes
  * 20 test students
  * 10 test assignments
  * Sample grades and posts
  
* ✅ Presentation deck:
  * Problem statement
  * Solution architecture
  * Technical implementation
  * Demo walkthrough
  * Q&A preparation

**Technical Documentation**:
* ✅ README.md with setup instructions
* ✅ Deployment guide (SAM commands)
* ✅ Environment configuration (.env template)
* ✅ Troubleshooting guide
* ✅ API changelog and versioning
  * DynamoDB operations: 80%
  * API Gateway integration: 75%
  
* ✅ **Test Frameworks Configured**:
  * Jest 29.5 with aws-sdk-client-mock
  * Supertest for API endpoint testing
  * DynamoDB Local for isolated testing
  * Cognito Local (cognito-local) for auth testing

* ✅ **Test Results**:
  * All 45 tests passing
  * Average test execution time: 3.2 seconds
  * CI/CD pipeline integration (GitHub Actions)
  * Automated test runs on every commit

**Lambda Optimization**:
* ✅ **Cold Start Reduction**:
  * Before: 1.2-1.8 seconds
  * After: 300-500ms (73% improvement)
  * Techniques: Provisioned concurrency (5 instances), Lambda layers, bundle optimization
  
* ✅ **Lambda Layers Created**:
  * aws-sdk-layer: 15MB (DynamoDB, S3, Cognito clients)
  * utils-layer: 2MB (shared utilities, validators)
  * Reduced individual function size by 60%
  
* ✅ **Connection Pooling**:
  * DynamoDB client reuse across invocations
  * Connection pool size: 10
  * Latency reduction: 40ms → 8ms per query
  
* ✅ **Bundle Optimization**:
  * Webpack tree-shaking enabled
  * Removed unused dependencies (lodash, moment → date-fns)
  * Bundle size: 8MB → 3MB (62% reduction)

**Security Enhancements**:
* ✅ **AWS WAF Configuration**:
  * Web ACL created with 5 rules:
    1. Rate limiting: 2000 requests per 5 minutes per IP
    2. Geo-blocking: Block traffic from high-risk countries
    3. SQL injection protection
    4. XSS protection
    5. Known bad inputs (OWASP Core Rule Set)
  * Cost: $5/month + $1 per million requests
  
* ✅ **API Gateway Security**:
  * Request validation enabled (JSON schema validation)
  * CORS configuration with whitelist domains
  * API keys for third-party integrations
  * Usage plans: 10,000 requests/day per key
  
* ✅ **Data Encryption**:
  * S3 bucket encryption: AWS KMS (CMK)
  * DynamoDB encryption at rest: AWS managed keys
  * Lambda environment variables: encrypted with KMS
  * API Gateway TLS 1.2 enforced
  
* ✅ **VPC Configuration**:
  * Lambda functions in private subnets
  * NAT Gateway for internet access (DynamoDB VPC endpoint)
  * Security groups: Allow outbound only to AWS services
  * VPC Flow Logs enabled

**API Documentation**:
* ✅ **OpenAPI 3.0 Specification Created**:
  * 13 endpoints documented with:
    - Request/response schemas
    - Authentication requirements
    - Example requests/responses
    - Error codes and messages
  * File size: 850 lines (YAML format)
  
* ✅ **Swagger UI Deployed**:
  * Hosted on S3 + CloudFront
  * URL: https://api-docs.student-management.com
  * Interactive API testing interface
  * Auto-generated code samples (cURL, JavaScript, Python)
  
* ✅ **Integration Guide**:
  * Authentication flow (Cognito JWT)
  * API endpoints with examples
  * Rate limiting and error handling
  * Polling mechanism for chat updates
  * Shared with frontend team via Notion

**Staging Environment**:
* ✅ **Infrastructure as Code (IaC)**:
  * AWS CDK TypeScript stack created
  * Separate stacks: dev, staging, production
  * Stack includes: Lambda, API Gateway, DynamoDB, Cognito, S3, EventBridge
  
* ✅ **CI/CD Pipeline**:
  * GitHub Actions workflow configured
  * Stages: Test → Build → Deploy to staging → Manual approval → Deploy to prod
  * Automated rollback on deployment failure
  * Blue-green deployment strategy

**Performance Benchmarks (After Optimization)**:
* API response time: 320ms → 180ms (44% improvement)
* Lambda cold starts: 1.5s → 400ms (73% improvement)
* DynamoDB query latency: 80ms → 15ms (with connection pooling)
* API Gateway cache hit rate: 72% → 85% (improved cache keys)
* Cost per 1M requests: $4.50 → $2.80 (38% reduction)

**Cost Analysis (Week 11)**:
* Lambda: $1.20 (with provisioned concurrency: +$0.80)
* DynamoDB: $0.95
* API Gateway: $0.45
* AWS WAF: $5.00 (fixed) + $0.20 (requests)
* CloudFront: $0.30 (Swagger UI hosting)
* KMS: $1.00 (customer managed keys)
* **Total: $9.90 USD**

**Key Learnings**:
* Lambda layers dramatically reduce cold start times and deployment package sizes
* Provisioned concurrency is cost-effective for high-traffic APIs (5+ req/sec)
* AWS WAF provides essential protection against common attacks
* Integration testing catches edge cases missed in unit tests
* OpenAPI documentation improves frontend-backend collaboration 
  * Compute
  * Storage
  * Networking 
  * Database
  * ...

* Successfully created and configured an AWS Free Tier account.

* Became familiar with the AWS Management Console and learned how to find, access, and use services via the web interface.

* Installed and configured AWS CLI on the computer, including:
  * Access Key
  * Secret Key
  * Default Region
  * ...

* Used AWS CLI to perform basic operations such as:

  * Check account & configuration information
  * Retrieve the list of regions
  * View EC2 service
  * Create and manage key pairs
  * Check information about running services
  * ...

* Acquired the ability to connect between the web interface and CLI to manage AWS resources in parallel.
* ...
