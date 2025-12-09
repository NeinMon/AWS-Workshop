---
title: "Week 10 Worklog"
date: "2024-11-18T00:00:00Z"
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---

### Week 10 Objectives:

* Implement REST API for Chat Messages
* Add Reactions feature (Like/Heart) for chat messages
* Implement Delete Comment API
* Optimize Lambda functions and API performance

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | ---- | ---------- | --------------- | ------------------ |
| 2 | - Design Chat Message schema: <br>&emsp; + Messages table (PK=message_id, GSI on class_id) <br>&emsp; + Attributes: class_id, author_id, content, reactions, created_at <br>&emsp; + Enable DynamoDB Streams for change tracking | 11/11/2025 | 11/11/2025 | [DynamoDB Streams](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Streams.html) |
| 3 | - Implement Chat REST APIs: <br>&emsp; + GET `/chat/messages/{class_id}` - List messages <br>&emsp; + POST `/chat/messages` - Send message <br>&emsp; + Polling mechanism for new messages <br>&emsp; + Support pagination | 12/11/2025 | 12/11/2025 | [Lambda with DynamoDB](https://docs.aws.amazon.com/lambda/latest/dg/with-ddb.html) |
| 4 | - Implement Reactions API: <br>&emsp; + PUT `/chat/messages/{id}/reactions` - Update Reactions <br>&emsp; + DynamoDB atomic counter (like_count, heart_count) <br>&emsp; + Track user reactions (prevent duplicates) | 13/11/2025 | 13/11/2025 | [DynamoDB Atomic Counters](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/WorkingWithItems.html#WorkingWithItems.AtomicCounters) |
| 5 | - Implement Delete Comment API: <br>&emsp; + DELETE `/comments/{id}` - Delete Comment <br>&emsp; + Validate author permissions <br>&emsp; + Soft delete (status=0) <br>&emsp; + Update comment count | 14/11/2025 | 14/11/2025 | [API Gateway Authorization](https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-authorization-flow.html) |
| 6 | - Lambda optimization & testing: <br> - Connection pooling for DynamoDB <br> - Lambda layers for shared code <br> - Test polling mechanism <br> - Performance testing (100 concurrent users) | 15/11/2025 | 15/11/2025 | [Lambda Best Practices](https://docs.aws.amazon.com/lambda/latest/dg/best-practices.html) |

### Week 10 Achievements:

**DynamoDB Setup for Chat Messages**:
* ✅ **Messages** table created:
  * PK: message_id (UUID)
  * Attributes: class_id, author_id, author_name, content, reactions (Map), created_at
  * GSI: class_id-index (PK=class_id, SK=created_at) for listing messages by class
  * TTL attribute: expires_at (optional - auto-delete messages after 90 days)
  
* ✅ **DynamoDB Streams** enabled:
  * Stream view type: NEW_AND_OLD_IMAGES
  * Lambda trigger for change tracking
  * Used for audit logging and analytics
  
* ✅ **Cognito Integration**:
  * JWT token validation for all endpoints
  * User enrollment check before message access

**APIs Implemented (4 Chat + Reactions + Delete Endpoints)**:
* ✅ **GET** `/chat/messages/{class_id}` - Get Chat Messages (REST API)
  * Query parameters: limit (default: 50), lastEvaluatedKey
  * Query Messages table using GSI (class_id-index)
  * Sort by created_at (ascending - oldest first)
  * Pagination: limit 50 messages per page
  * Returns: `{ messages: [...], lastEvaluatedKey }`
  * Latency: ~120ms
  
* ✅ **POST** `/chat/messages` - Send Chat Message (REST API)
  * Request body: `{ class_id, content }`
  * Validate user is enrolled in class
  * Write to Messages table (message_id, class_id, author_id, content, created_at)
  * **Trigger DynamoDB Stream**: Change captured for analytics
  * Returns: message_id, created_at
  * Latency: ~80ms (write)
  
* ✅ **PUT** `/chat/messages/{id}/reactions` - Update Reactions (Like/Heart)
  * Request body: `{ reaction_type: "like" | "heart" }`
  * DynamoDB atomic update:
    * If user not in `liked_by`: Increment `like_count`, append user_id to `liked_by`
    * If user already liked: Decrement `like_count`, remove user_id (toggle)
  * Use `UpdateExpression`: `SET reactions.like_count = reactions.like_count + :val`
  * Prevent duplicate reactions from same user
  * Returns: updated reaction counts `{ like_count, heart_count }`
  * Latency: ~60ms
  
* ✅ **DELETE** `/comments/{id}` - Delete Comment
  * Path parameter: comment_id
  * Validate user is comment author OR teacher of class
  * Query PostComments table to get post_id
  * Soft delete: Update status=0 in PostComments table
  * Atomic decrement comment_count in Posts table
  * Returns: `{ message: "Comment deleted successfully" }`
  * Latency: ~90ms

**Message Polling Flow**:
1. Client calls GET `/chat/messages/{class_id}` periodically (every 3-5 seconds)
2. Query includes `lastMessageTimestamp` parameter
3. API returns only new messages after that timestamp
4. Client updates UI with new messages
5. **Polling interval**: 3 seconds (configurable)
6. **Optimization**: Client-side caching to reduce API calls


**Lambda Optimization**:
* ✅ Connection pooling implemented:
  * DynamoDB client reuse across invocations
  * Reduced latency: 80ms → 15ms per query
  
* ✅ Lambda layers created:
  * aws-sdk-layer: 12MB (DynamoDB clients)
  * utils-layer: 1.5MB (shared utilities)
  * Reduced individual function size by 65%
  
* ✅ Bundle optimization:
  * Webpack tree-shaking enabled
  * Bundle size: 6MB → 2.5MB
  * Cold start time: 1.8s → 600ms (67% improvement)

**Testing Results**:
* ✅ Chat polling mechanism: 100 concurrent users, 3-second polling interval
* ✅ Chat message latency: 80ms avg (write)
* ✅ Message retrieval: 120ms avg latency (with pagination)
* ✅ Reactions API: 60ms avg latency (atomic update)
* ✅ Delete Comment: 90ms avg latency
* ✅ Load test: 500 messages sent over 10 minutes, 0 errors

**Monitoring Infrastructure**:
* ✅ CloudWatch dashboards created:
  * **API Performance**: Request count, latency (p50, p95, p99), errors
  * **Lambda Metrics**: Invocations, duration, errors, concurrent executions
  * **DynamoDB Metrics**: Read/write capacity, throttles
  * **Cost Dashboard**: Daily spending by service
  
* ✅ CloudWatch Alarms configured (8 alarms):
  1. API Gateway 5xx errors > 10 in 5 minutes
  2. Lambda error rate > 5%
  3. DynamoDB throttles > 50 in 5 minutes
  4. API latency p99 > 2 seconds
  5. Lambda concurrent executions > 800
  6. Daily cost > $20
  7. S3 bucket storage > 50GB
  8. Cognito authentication failures > 20 in 5 minutes
  
* ✅ AWS X-Ray tracing enabled:
  * All Lambda functions instrumented
  * Service map visualization
  * Identify bottlenecks (DynamoDB queries, S3 uploads)

**End-to-End Testing**:
* ✅ All 11 APIs tested:
  * 5 Class Management APIs
  * 5 Assignment Management APIs
  * 1 Post API
* ✅ Test scenarios:
  * Create class → Add assignment → Update grades → Post announcement
  * List classes → List assignments → List students
  * Edit/Delete operations
* ✅ Authentication flow validated
* ✅ File upload/download tested
* ✅ Notification delivery confirmed

**Performance Metrics**:
* API response time: 280ms → 180ms (average)
* Lambda cold start: 1.8s → 600ms
* DynamoDB query latency: 80ms → 15ms
* Cache hit rate: 68%
* Error rate: < 0.1%

**Cost Estimate (Production)**:
* Lambda: ~$5-8/month (1M requests)
* DynamoDB: ~$3-5/month (on-demand)
* API Gateway: ~$3-4/month
* S3: ~$1-2/month (10GB storage)
* CloudWatch: ~$2-3/month
* SNS: ~$0.50/month
* **Total: ~$15-23/month** (for 100-200 active users)
* ✅ Model: aws-sims recipe, training time 2.5 hours
* ✅ Metrics: Precision@5: 0.73, Recall@5: 0.68, Coverage: 0.85
* ✅ Lambda integration with GetRecommendations API

**DynamoDB Optimization**:
* ✅ **ClassRankingIndex GSI**: PK: classId, SK: score
  * Query time: 1.2s → 80ms (93% reduction)
  * RCU usage: 1000 → 5 (99.5% reduction)
* ✅ **TopPerformersIndex** (Sparse GSI): score > 80 only
  * Index size reduced by 70%

**Real-Time Notifications**:
* ✅ EventBridge rule triggers on score updates
* ✅ End-to-end latency: 1.5 seconds (score update → notification)
* ✅ Polling-based notification delivery

**Performance Testing Results**:
* ✅ Load test: 500 concurrent users, 100 req/sec for 30 minutes
* ✅ Average response time: 320ms
* ✅ 95th percentile: 650ms
* ✅ Error rate: 0.02% (timeouts only)
* ✅ API Gateway cache hit rate: 72%

**CloudWatch Dashboards**:
* ✅ Created "Ranking Analytics" dashboard with metrics:
  * Lambda invocations, duration, errors
  * API Gateway latency, cache hit rate
  * DynamoDB throttles, read/write capacity

**Cost Analysis (Week 10)**:
* Lambda: $1.40 (~3M invocations with polling)
* DynamoDB: $1.20 (increased reads from polling)
* API Gateway: $0.60
* CloudWatch: $0.25
* **Total: $3.45 USD**

**Key Learnings**:
* Polling interval balance: Too frequent = high cost, too slow = poor UX
* DynamoDB GSI design critical for efficient message queries
* API Gateway caching reduces Lambda invocations by 70%
* Client-side caching essential for polling approach
* DynamoDB Streams useful for audit logging and analytics
* Load testing reveals bottlenecks not visible in development
