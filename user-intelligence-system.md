# User Intelligence System
> One backend powering both real-time context and analytics insights

## 🎯 Core Insight
Analytics and user memory are the same problem - understanding what users want. Build once, use everywhere.

## 🏗️ Architecture

```
User Chats → Platform API → PostgreSQL (Raw Storage)
                ↓
         Event Stream (Redis)
                ↓
    ┌───────────┴───────────┐
    ↓                       ↓
Real-time Context      Batch Analytics
(User Memory)          (Product Insights)
    ↓                       ↓
Chat Experience        PMF Dashboard
```

## 📊 Unified Data Model

### Core Tables

```sql
-- Raw conversation storage
CREATE TABLE conversations (
    id UUID PRIMARY KEY,
    user_id TEXT NOT NULL,
    started_at TIMESTAMP DEFAULT NOW(),
    last_message_at TIMESTAMP,
    message_count INT DEFAULT 0,
    credits_used INT DEFAULT 0,
    
    -- Metadata
    first_intent TEXT, -- What they asked for initially
    tools_used JSONB, -- {image_gen: 5, video_gen: 2, search: 10}
    completion_status TEXT, -- completed, abandoned, blocked_by_credits
    
    -- Computed fields (updated by pipeline)
    user_satisfaction_signal INT, -- 0-10 based on behavior
    extracted_use_case TEXT, -- "marketing_content", "personal_video", etc
    blocker_reason TEXT -- what stopped them
);

-- Individual messages/events
CREATE TABLE conversation_events (
    id UUID PRIMARY KEY,
    conversation_id UUID REFERENCES conversations(id),
    user_id TEXT NOT NULL,
    timestamp TIMESTAMP DEFAULT NOW(),
    event_type TEXT, -- message, tool_call, error, completion
    
    -- Flexible content
    content JSONB, -- {role, message, tool, params, result, error}
    
    -- Indexable fields for fast queries
    tool_name TEXT,
    success BOOLEAN,
    credits_consumed INT,
    
    INDEX idx_user_events (user_id, timestamp),
    INDEX idx_tool_usage (tool_name, timestamp)
);

-- User profile (built from conversations)
CREATE TABLE user_profiles (
    user_id TEXT PRIMARY KEY,
    email TEXT,
    created_at TIMESTAMP,
    
    -- Engagement metrics
    total_conversations INT DEFAULT 0,
    total_messages INT DEFAULT 0,
    total_credits_used INT DEFAULT 0,
    last_active_at TIMESTAMP,
    
    -- Behavioral profile
    primary_use_case TEXT, -- Inferred from usage
    preferred_tools JSONB, -- Most used tools
    average_session_length INT, -- minutes
    completion_rate FLOAT, -- % of conversations completed
    
    -- PMF signals
    retention_cohort TEXT, -- day_1, week_1, week_2, churned
    activation_status TEXT, -- signed_up, first_creation, power_user, dormant
    credit_exhaustion_count INT, -- How many times hit limit
    
    -- Memory/Context (for real-time)
    preferences JSONB, -- {style: "cinematic", tone: "professional"}
    context_summary TEXT, -- LLM-generated summary of past interactions
    frequently_used_prompts TEXT[] -- Common patterns
);
```

## 🔄 Data Pipeline

### 1. Real-time Stream (for user context)
```javascript
// In Platform API - on every message
async function processMessage(userId, message, toolCalls, result) {
    // Store raw event
    await storeConversationEvent({
        userId,
        type: 'message',
        content: { message, toolCalls, result }
    });
    
    // Update conversation metadata
    await updateConversation(conversationId, {
        lastMessageAt: now(),
        messageCount: increment(),
        toolsUsed: appendTools(toolCalls)
    });
    
    // Emit for real-time context
    await redis.publish('user_context', {
        userId,
        event: 'new_interaction',
        data: { message, toolCalls }
    });
}
```

### 2. Batch Analytics (hourly/daily)
```sql
-- Key queries for PMF signals

-- 1. Activation Funnel
WITH user_journey AS (
    SELECT 
        user_id,
        MIN(started_at) as first_conversation,
        COUNT(DISTINCT DATE(started_at)) as active_days,
        SUM(credits_used) as total_credits,
        MAX(CASE WHEN tools_used ? 'video_gen' THEN 1 ELSE 0 END) as created_video,
        MAX(CASE WHEN completion_status = 'completed' THEN 1 ELSE 0 END) as completed_any
    FROM conversations
    GROUP BY user_id
)
SELECT 
    COUNT(*) as total_users,
    SUM(CASE WHEN active_days >= 1 THEN 1 ELSE 0 END) as day_1_active,
    SUM(CASE WHEN created_video = 1 THEN 1 ELSE 0 END) as created_content,
    SUM(CASE WHEN total_credits >= 90 THEN 1 ELSE 0 END) as high_usage,
    SUM(CASE WHEN completed_any = 1 THEN 1 ELSE 0 END) as successful_users
FROM user_journey;

-- 2. Intent Extraction (What are users trying to do?)
SELECT 
    DATE(timestamp) as day,
    content->>'message' as user_message,
    tool_name,
    success,
    COUNT(*) as frequency
FROM conversation_events
WHERE event_type = 'message'
GROUP BY 1, 2, 3, 4
ORDER BY frequency DESC;

-- 3. Failure Points (Where do users get stuck?)
SELECT 
    tool_name,
    content->>'error' as error_type,
    COUNT(*) as failure_count,
    COUNT(DISTINCT user_id) as affected_users
FROM conversation_events
WHERE success = false
GROUP BY 1, 2
ORDER BY failure_count DESC;

-- 4. Credit Exhaustion (PMF signal - wanting more)
SELECT 
    user_id,
    email,
    total_credits_used,
    credit_exhaustion_count,
    last_active_at
FROM user_profiles
WHERE total_credits_used >= 90
ORDER BY credit_exhaustion_count DESC;
```

## 📈 PMF Dashboard Views

### 1. Executive Summary
```
Daily Active Users: 23/100 (23%)
Week 1 Retention: 8/15 (53%)  
Credits Exhausted: 5 users (🔥 HOT SIGNAL)
Avg Session Length: 12 minutes

Top Use Cases:
1. Marketing content (34%)
2. Personal videos (28%)
3. Social media clips (21%)
```

### 2. User Journey Heatmap
```
Signed Up → First Chat → Used Tool → Created Content → Exhausted Credits
  100     →    67     →    45    →      23       →        5
         (67%)       (67%)      (51%)           (22%)
```

### 3. Intent Clusters (LLM-analyzed)
```python
async def extractUserIntent(conversations):
    prompt = """
    Analyze these user conversations and extract:
    1. What they're trying to accomplish
    2. Where they get stuck
    3. What features they're asking for that don't exist
    
    Conversations: {conversations}
    
    Return as JSON: {
        primary_intent: str,
        blockers: [str],
        feature_requests: [str],
        satisfaction_signal: 0-10
    }
    """
    return await llm.analyze(prompt, conversations)
```

## 🔮 Implementation Plan

### Phase 1: Basic Tracking (Day 1)
- [x] Create database schema
- [ ] Add event logging to Platform API
- [ ] Simple SQL queries for daily metrics
- [ ] Basic CLI tool for viewing stats

### Phase 2: Real-time Context (Day 2-3)
- [ ] Redis pub/sub for events
- [ ] User profile builder
- [ ] Context API endpoint
- [ ] Integrate with chat for personalization

### Phase 3: Intelligence Layer (Day 4-5)
- [ ] LLM batch analysis job
- [ ] Intent clustering
- [ ] Automated insights email
- [ ] Feature request extraction

### Phase 4: PMF Dashboard (Day 6-7)
- [ ] Web dashboard (simple HTML)
- [ ] Real-time metrics
- [ ] User drill-down views
- [ ] Export for investor updates

## 🎯 Success Metrics

### Strong PMF Signals:
- Credit exhaustion rate > 20%
- Day 7 retention > 40%
- Organic sharing/invites
- "I love this" in conversations

### Weak PMF Signals:
- Single session users > 50%
- No credit exhaustion
- Conversations < 5 messages
- High error rates

### Pivot Signals:
- Same blockers repeatedly
- Users asking for completely different features
- Use case mismatch with our vision

## 🚀 Quick Start

```bash
# 1. Run migrations
cd latent-platform-api-v2
go run migrations/add_analytics_tables.go

# 2. Start tracking
# (Already happens via Platform API)

# 3. View analytics
go run cmd/analytics/main.go --today

# 4. Get AI insights
go run cmd/analytics/main.go --ai-analysis
```

## 💡 Key Insight
Every conversation is both:
1. **Context** for better real-time experience
2. **Data** for product development

Build the infrastructure once, benefit twice.