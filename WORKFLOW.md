# 📋 TubeVer Beta 0.1 - Workflow & Process

**Version:** Beta 0.1  
**Last Updated:** April 2026  
**Status:** Development Phase

---

## Table of Contents

1. [Development Workflow](#development-workflow)
2. [System Architecture Flow](#system-architecture-flow)
3. [User Flows](#user-flows)
4. [Technical Implementation Flow](#technical-implementation-flow)
5. [Deployment Pipeline](#deployment-pipeline)
6. [Testing & QA Process](#testing--qa-process)

---

## Development Workflow

### Phase 1: Planning & Design
```
┌─────────────────┐
│  Requirement    │
│  Analysis       │
└────────┬────────┘
         ↓
┌─────────────────────────────────┐
│  Design System & UI/UX          │
│  - Figma Mockups                │
│  - User Journey Maps            │
│  - Component Library             │
└────────┬────────────────────────┘
         ↓
┌──────────────────────────────────┐
│  API & Database Schema Design    │
│  - ER Diagram                    │
│  - API Endpoint Specification    │
└────────┬─────────────────────────┘
         ↓
┌──────────────────────────────────┐
│  Technical Specification Ready   │
└──────────────────────────────────┘
```

### Phase 2: Backend Development (Golang)
```
┌──────────────────────────────────────┐
│  1. Core API Development             │
│  ├─ Authentication Service           │
│  ├─ Verification Service             │
│  ├─ Calculation Engine                │
│  ├─ Campaign Management API           │
│  ├─ Rule Engine API                   │
│  ├─ Link Generator Service            │
│  ├─ Execution/Raffle Service          │
│  └─ Result Recording Service          │
└────────┬─────────────────────────────┘
         ↓
┌──────────────────────────────────────┐
│  2. Database Setup                   │
│  ├─ PostgreSQL Schema Creation       │
│  ├─ Migration Scripts                │
│  └─ Initial Data Seeding             │
└────────┬─────────────────────────────┘
         ↓
┌──────────────────────────────────────┐
│  3. Cache & Queue Implementation     │
│  ├─ Redis Configuration              │
│  ├─ Queue Workers Setup              │
│  └─ Rate Limiting Logic              │
└────────┬─────────────────────────────┘
         ↓
┌──────────────────────────────────────┐
│  4. External Integration             │
│  └─ YouTube Data API v3 Integration  │
└──────────────────────────────────────┘
```

### Phase 3: Frontend Development (Next.js)
```
┌────────────────────────────────────────┐
│  1. Project Setup & Tooling            │
│  ├─ Next.js Configuration              │
│  ├─ TypeScript Setup                   │
│  ├─ Dark/Light Mode System             │
│  └─ Component Architecture             │
└────────┬───────────────────────────────┘
         ↓
┌────────────────────────────────────────┐
│  2. Core Pages & Components            │
│  ├─ Authentication Pages               │
│  ├─ Gacha Wheel UI                     │
│  ├─ Creator Dashboard                  │
│  ├─ Campaign Management Pages          │
│  ├─ Rule Configuration UI              │
│  └─ Result Display Pages               │
└────────┬───────────────────────────────┘
         ↓
┌────────────────────────────────────────┐
│  3. Animation Implementation            │
│  ├─ Framer Motion Setup                │
│  ├─ Gacha Wheel Animation              │
│  ├─ Transition Effects                 │
│  └─ Micro-interactions                 │
└────────┬───────────────────────────────┘
         ↓
┌────────────────────────────────────────┐
│  4. API Integration                    │
│  ├─ Backend API Connection             │
│  ├─ State Management (Redux/Zustand)   │
│  └─ Error Handling & Loading States    │
└────────────────────────────────────────┘
```

---

## System Architecture Flow

### Authentication & Authorization
```
User Click Campaign Link
        ↓
YouTube OAuth Login
        ↓
Authorization Code Received
        ↓
Frontend → Backend API
        ↓
Exchange Code for JWT Token
        ↓
Store Token (Secure Cookie/LocalStorage)
        ↓
User Authenticated ✓
```

### Campaign Participation Flow
```
┌─────────────────────────────────────────┐
│  1. AUTHENTICATION PHASE                │
│  └─ User logs in via YouTube OAuth      │
└────────┬────────────────────────────────┘
         ↓
┌─────────────────────────────────────────┐
│  2. VERIFICATION PHASE                  │
│  └─ Backend fetches YouTube subscription│
│     data via Data API v3                │
│     (Channel ID, Membership Tier,       │
│      Duration, Subscriber Status)       │
└────────┬────────────────────────────────┘
         ↓
┌─────────────────────────────────────────┐
│  3. CALCULATION PHASE                   │
│  └─ Rule Engine applies Creator's rules │
│     to determine Weight/Multiplier:     │
│     • Regular Subscriber = 1x           │
│     • Tier 1 Member = 1.5x              │
│     • Tier 2 Member = 2x                │
│     • Tier 3 Member = 3x                │
│     • Duration Bonus = +0.5x per month  │
└────────┬────────────────────────────────┘
         ↓
┌─────────────────────────────────────────┐
│  4. EXECUTION PHASE                     │
│  ├─ User clicks "Spin Wheel"            │
│  ├─ Backend processes raffle with       │
│  │  weighted random selection           │
│  ├─ Result recorded to PostgreSQL       │
│  ├─ Result sent to Frontend             │
│  └─ Wheel animates to result (Framer)   │
└────────┬────────────────────────────────┘
         ↓
┌─────────────────────────────────────────┐
│  5. RESULT PHASE                        │
│  ├─ Display reward to user              │
│  ├─ Log event to audit trail            │
│  └─ Update user's reward history        │
└─────────────────────────────────────────┘
```

---

## User Flows

### 👀 Viewer User Flow

```
START (Click Campaign Link)
    ↓
[Is Authenticated?]
├─ No → YouTube OAuth Login → Get JWT
└─ Yes → Skip Auth
    ↓
[Load Campaign Details]
├─ Campaign Title, Description
├─ Prize Pool
├─ Rules & Eligibility
└─ Current Wheel State
    ↓
[Display Eligibility Status]
├─ Subscription Level: Show "Regular" or "Member Tier X"
├─ Duration: Show "X months supporter"
├─ Weight/Multiplier: Show "Your odds multiplier: X.X"
└─ Allow/Block Spin Button
    ↓
[Check Eligibility & Limits]
├─ Already participated today?
├─ Within entry limit?
└─ Membership status valid?
    ↓
[User Decides]
├─ Spin Wheel → Go to Execution
└─ Exit → End Flow
    ↓
[EXECUTION]
├─ Frontend sends spin request
├─ Backend calculates result with weighted probability
├─ Animate wheel to result
├─ Display reward
└─ Show reward details & claim options
    ↓
[Result Displayed]
├─ "Congratulations! You won [PRIZE]"
├─ Option to share result
└─ View history/leaderboard
    ↓
END
```

### 🎬 Creator User Flow

```
START (Creator Dashboard Login)
    ↓
[Creator Authentication]
└─ Login via email/OAuth
    ↓
[Main Dashboard]
├─ View Active Campaigns
├─ View Campaign Statistics
├─ Create New Campaign
└─ Access Settings
    ↓
[IF: Create New Campaign]
├─ Step 1: Basic Campaign Info
│  ├─ Campaign Name
│  ├─ Description
│  ├─ Duration (Start/End Date)
│  └─ Campaign Status (Draft/Live)
│
├─ Step 2: Prize Pool Configuration
│  ├─ Define Prize Tiers
│  ├─ Set Prize Distribution %
│  ├─ Upload Prize Images/Details
│  └─ Set Prize Values (if applicable)
│
├─ Step 3: Rule Engine Setup
│  ├─ Define Eligibility Criteria
│  │  ├─ Regular Subscriber: 1x weight
│  │  ├─ Tier 1 Member: 1.5x weight
│  │  ├─ Tier 2 Member: 2x weight
│  │  ├─ Tier 3 Member: 3x weight
│  │  └─ Duration Multiplier: +0.5x per month
│  │
│  ├─ Set Limits
│  │  ├─ Max Spins Per User
│  │  ├─ Min Subscriber Duration
│  │  └─ Member Tier Requirements
│  │
│  └─ Advanced Rules (Optional)
│     ├─ Custom eligibility conditions
│     └─ Special blacklist/whitelist
│
├─ Step 4: Generate Campaign Link
│  ├─ System generates unique URL
│  ├─ QR Code generated
│  ├─ Copy to clipboard
│  ├─ Get share templates for YouTube
│  └─ Pin in chat instructions
│
└─ Step 5: Review & Launch
   ├─ Preview campaign
   ├─ Test with staging account
   └─ Launch Live ✓
    ↓
[Campaign Live]
├─ Real-time participant tracking
├─ View live participant list
├─ Monitor results & statistics
├─ View winner's usernames
└─ Export results
    ↓
[Campaign Management]
├─ Pause/Resume Campaign
├─ View detailed analytics
│  ├─ Total Participants
│  ├─ Total Spins
│  ├─ Prize Distribution
│  ├─ Membership breakdown
│  └─ Engagement metrics
│
├─ Edit Campaign (if in draft)
└─ Archive Campaign (when ended)
    ↓
END (Campaign Results Stored)
```

---

## Technical Implementation Flow

### Backend (Golang) - 8 Core Functions

```
1. AUTHENTICATE_USER(code: string) → JWT
   ├─ Receive OAuth authorization code
   ├─ Exchange code for access token (Google)
   ├─ Extract user info (Channel ID, Display Name)
   ├─ Generate/return JWT token
   └─ Return: {jwt, user_id, channel_id}

2. VERIFY_SUBSCRIPTION(user_id, channel_id, creator_channel_id) → SubscriptionData
   ├─ Call YouTube Data API v3
   ├─ Check subscription status
   ├─ Get membership tier (None/Tier1/Tier2/Tier3)
   ├─ Calculate membership duration
   └─ Return: {is_subscriber, tier, duration_months}

3. CALCULATE_WEIGHT(subscription_data, rules) → Weight
   ├─ Load campaign rules from DB
   ├─ Apply base weight by subscription level
   ├─ Apply duration multiplier
   ├─ Apply custom rule conditions
   ├─ Cache result in Redis
   └─ Return: {weight: float64}

4. GET_CAMPAIGN_PRIZES(campaign_id) → PrizeList
   ├─ Query PostgreSQL for campaign data
   ├─ Retrieve prize pool with weights/probabilities
   ├─ Return: {prize_id, name, description, probability}[]

5. EXECUTE_RAFFLE(campaign_id, user_id, weight) → WinnerPrize
   ├─ Acquire lock in Redis (prevent double-spin)
   ├─ Perform weighted random selection
   │  ├─ Generate random number
   │  ├─ Apply user's weight multiplier
   │  └─ Select prize by probability
   ├─ Insert result to PostgreSQL
   ├─ Push event to queue (Redis)
   ├─ Update cache
   └─ Return: {prize_id, prize_name, is_winner}

6. GENERATE_CAMPAIGN_LINK(campaign_id, creator_id) → URL
   ├─ Validate creator authorization
   ├─ Generate unique campaign token
   ├─ Create shortened URL in DB
   ├─ Generate QR code data
   └─ Return: {full_url, qr_code_data}

7. GET_CAMPAIGN_STATS(campaign_id) → Statistics
   ├─ Query participant counts
   ├─ Calculate participation rate
   ├─ Aggregate prize distribution
   ├─ Analyze subscription breakdown
   ├─ Generate reports
   └─ Return: {total_participants, total_spins, analytics}

8. RECORD_RESULT(spin_id, result_data) → Success
   ├─ Persist result to PostgreSQL
   ├─ Update user's result history
   ├─ Trigger audit logging
   ├─ Update campaign metrics cache
   └─ Return: {recorded: true, result_id}
```

### Frontend (Next.js) - Page Structure

```
/
├─ index.tsx (Landing page)
├─ auth/
│  ├─ login.tsx (YouTube OAuth redirect)
│  └─ callback.tsx (OAuth callback handler)
├─ campaign/
│  ├─ [id].tsx (Campaign spin page)
│  ├─ [id]/result.tsx (Result display)
│  └─ [id]/history.tsx (User's spin history)
├─ dashboard/
│  ├─ index.tsx (Creator dashboard home)
│  ├─ campaigns/
│  │  ├─ new.tsx (Create campaign)
│  │  ├─ [id].tsx (Campaign detail/edit)
│  │  ├─ [id]/analytics.tsx (Analytics page)
│  │  └─ [id]/participants.tsx (Participant list)
│  └─ settings.tsx (Creator settings)
└─ api/
   ├─ auth/ (Auth endpoints)
   ├─ campaigns/ (Campaign endpoints)
   ├─ verify/ (Verification endpoints)
   └─ results/ (Results endpoints)
```

### Database Schema (PostgreSQL)

```
TABLES:
├─ users
│  ├─ id (PK)
│  ├─ channel_id (YouTube channel ID)
│  ├─ display_name
│  ├─ email
│  ├─ role (viewer/creator)
│  ├─ created_at
│  └─ updated_at
│
├─ campaigns
│  ├─ id (PK)
│  ├─ creator_id (FK: users)
│  ├─ name
│  ├─ description
│  ├─ rules_json (Rule Engine config)
│  ├─ status (draft/live/ended)
│  ├─ start_date
│  ├─ end_date
│  ├─ created_at
│  └─ updated_at
│
├─ prizes
│  ├─ id (PK)
│  ├─ campaign_id (FK: campaigns)
│  ├─ name
│  ├─ description
│  ├─ probability (%)
│  ├─ weight (relative value)
│  ├─ image_url
│  ├─ order
│  └─ updated_at
│
├─ results (Spin Results)
│  ├─ id (PK)
│  ├─ campaign_id (FK: campaigns)
│  ├─ user_id (FK: users)
│  ├─ prize_id (FK: prizes)
│  ├─ user_weight (calculated)
│  ├─ random_value
│  ├─ created_at
│  └─ metadata_json
│
└─ audit_logs
   ├─ id (PK)
   ├─ event_type
   ├─ user_id
   ├─ campaign_id
   ├─ data_json
   ├─ ip_address
   ├─ timestamp
   └─ status
```

### API Endpoints

```
AUTH ENDPOINTS:
POST   /api/auth/youtube-login          → {redirect_url}
POST   /api/auth/callback               → {jwt, user_id}
POST   /api/auth/logout                 → {success: true}

VERIFICATION ENDPOINTS:
GET    /api/verify/subscription         → {tier, duration, is_valid}

CAMPAIGN ENDPOINTS:
GET    /api/campaigns                   → {campaigns[]}
POST   /api/campaigns                   → {campaign_id}
GET    /api/campaigns/{id}              → {campaign_data}
PUT    /api/campaigns/{id}              → {success}
DELETE /api/campaigns/{id}              → {success}

CAMPAIGN LINK ENDPOINTS:
POST   /api/campaigns/{id}/generate-link → {url, qr_code}
GET    /api/{short_code}                → Redirect to campaign

EXECUTION ENDPOINTS:
GET    /api/campaigns/{id}/prizes       → {prizes[]}
POST   /api/campaigns/{id}/spin         → {prize, result_id}

ANALYTICS ENDPOINTS:
GET    /api/campaigns/{id}/stats        → {analytics}
GET    /api/campaigns/{id}/results      → {results[]}
```

---

## Deployment Pipeline

### Local Development
```
git clone tubever
cd tubever
make setup                    # Install dependencies
make dev                      # Run frontend + backend locally
make test                     # Run unit tests
make lint                     # Code quality checks
```

### Staging Deployment
```
1. Create feature branch
   git checkout -b feature/xyz

2. Develop and test locally
   make test
   make lint

3. Push to repository
   git push origin feature/xyz

4. Create Pull Request (code review)

5. Automated CI Pipeline runs:
   ├─ Run all tests
   ├─ Code quality scan
   ├─ Build Docker images
   └─ Deploy to staging environment

6. QA Testing on staging
   ├─ Functional testing
   ├─ Performance testing
   └─ Integration testing

7. Approval & Merge to main
   git merge --squash
```

### Production Deployment
```
1. Tag release
   git tag v0.1.0-beta

2. Push tag
   git push origin v0.1.0-beta

3. CD Pipeline triggers:
   ├─ Build optimized Docker images
   ├─ Run smoke tests
   ├─ Deploy to production
   │  ├─ Database migrations
   │  ├─ Backend service update
   │  └─ Frontend CDN update
   ├─ Health checks
   └─ Monitoring alerts enabled

4. Post-deployment:
   ├─ Verify all endpoints
   ├─ Monitor error rates
   ├─ Check performance metrics
   └─ Ready for user traffic
```

### Infrastructure Stack
```
Frontend:
├─ Next.js app → Vercel / AWS Amplify
├─ Static assets → CDN (CloudFlare/AWS CloudFront)
└─ SSL/TLS: Automatic

Backend:
├─ Golang API → Docker container → Kubernetes/ECS
├─ Auto-scaling: 2-10 instances based on load
└─ Load balancer: Nginx/AWS ALB

Database:
├─ PostgreSQL: AWS RDS (Multi-AZ)
├─ Backup: Automated daily snapshots
└─ Failover: 99.95% uptime SLA

Cache:
├─ Redis: AWS ElastiCache
├─ Cluster mode: Enabled
└─ Persistence: AOF enabled

Monitoring:
├─ CloudWatch / DataDog
├─ Error tracking: Sentry
├─ Performance: New Relic
└─ Uptime: StatusPage
```

---

## Testing & QA Process

### Unit Testing
```
Backend (Golang):
├─ Test all 8 core functions
├─ Test weight calculation logic
├─ Test rule engine conditions
├─ Test error handling
└─ Coverage target: >85%

Frontend (React/Next.js):
├─ Test component rendering
├─ Test user interactions
├─ Test state management
├─ Test animation states
└─ Coverage target: >80%
```

### Integration Testing
```
End-to-End Flows:
├─ User Auth Flow
│  ├─ OAuth login
│  ├─ Token generation
│  └─ Session persistence
│
├─ Participation Flow
│  ├─ Eligibility verification
│  ├─ Weight calculation
│  ├─ Raffle execution
│  └─ Result display
│
├─ Campaign Management Flow
│  ├─ Campaign creation
│  ├─ Rule configuration
│  ├─ Link generation
│  └─ Analytics retrieval
│
└─ YouTube API Integration
   ├─ Authentication
   ├─ Subscription verification
   └─ Data accuracy
```

### Performance Testing
```
Load Tests:
├─ 1000 concurrent users spinning wheel
├─ Response time target: <200ms
├─ Success rate: 99.9%
└─ Database connection pool: Monitor max_connections

Stress Tests:
├─ Sustained load for 1 hour
├─ Ramp-up: 100 users/minute
└─ Graceful degradation: Queue overflow handling

Frontend Performance:
├─ Lighthouse score: >90
├─ First Contentful Paint (FCP): <1.5s
├─ Cumulative Layout Shift (CLS): <0.1
└─ Animation FPS: 60fps constant
```

### Security Testing
```
├─ OWASP Top 10 vulnerability scan
├─ SQL injection prevention verification
├─ XSS attack prevention
├─ CSRF token validation
├─ JWT expiration & refresh logic
├─ Rate limiting effectiveness
├─ API endpoint authentication
└─ Data encryption in transit & at rest
```

---

## Success Metrics (Beta 0.1)

```
🎯 Functional Metrics:
├─ 100% core features working
├─ All 8 API functions operational
├─ YouTube API integration successful
├─ Database transactions reliable
└─ Campaign link generation working

📊 Performance Metrics:
├─ API response time: <200ms (p95)
├─ Wheel animation: 60fps
├─ Database query time: <50ms (p95)
├─ Concurrent users: 1000+
└─ Cache hit ratio: >80%

👥 User Experience Metrics:
├─ Successful campaign creation: 99%
├─ Successful spin execution: 99%
├─ Page load time: <2s
├─ Animation smoothness: 60fps
└─ User satisfaction score: >4.5/5

🔒 Security Metrics:
├─ Zero security vulnerabilities (OWASP)
├─ All endpoints authenticated
├─ Rate limiting active
├─ Audit logs complete
└─ Encryption enabled
```

---

## Release Checklist (Beta 0.1)

- [ ] All features implemented and tested
- [ ] Code review completed
- [ ] Automated tests passing (unit + integration)
- [ ] Performance benchmarks met
- [ ] Security audit passed
- [ ] Documentation complete
- [ ] User onboarding guide ready
- [ ] Support team trained
- [ ] Monitoring & alerting configured
- [ ] Backup & recovery procedures verified
- [ ] Staging environment sign-off
- [ ] Production deployment ready
- [ ] Post-launch monitoring plan in place

---

**Next Phase:** Beta 0.2 (Enhanced features, performance optimization, additional payment integrations)
