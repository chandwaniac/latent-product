# Analytics Pipeline Design

## Problem Statement

**Core Challenge**: Understanding product-market fit signals from 100 beta users without being creepy or invasive.

**Key Questions**:
1. Are users getting value? (behavior > words)
2. What are they trying to accomplish? (use cases)
3. Where do they get stuck? (friction points)
4. Will they pay? (credit exhaustion, return rate)

**Constraints**:
- Can't manually read all conversations (privacy + scale)
- Traditional analytics miss nuance (pageviews ≠ value)
- Arbitrary scoring/metrics are meaningless
- Must work from day 1 to 10,000 users

## Solution: LLM-Native Analytics

**Core Insight**: We already store everything in `conversations`, `messages`, and `service_jobs`. LLMs can read this unstructured data better than any SQL query.

## Architecture

```
Raw Data (PostgreSQL) → Go Pipeline → LLM Analysis → Insights
     ↓                      ↓              ↓            ↓
[conversations]     [batch processor]  [Gemini]   [summary]
[messages]          [context builder]             [patterns]
[service_jobs]      [rate limiting]              [segments]
```

## Design Principles

1. **Store Everything, Structure Nothing**
   - Conversations are the source of truth
   - Add minimal structured data (downloads, shares)
   - Let LLMs find patterns we didn't anticipate

2. **Batch Over Real-time**
   - Daily analysis is sufficient for PMF
   - Reduces API costs and complexity
   - Can reprocess historical data as needed

3. **Progressive Enhancement**
   - Start with your data (user #0)
   - Add tracking only for what's missing
   - Scale gradually as patterns emerge

## Implementation (Go)

### Phase 1: Analyze Existing Data
```go
// internal/analytics/analyzer.go
type UserAnalysis struct {
    UserID           string
    TotalConversations int
    AverageDepth     float64
    ToolUsagePattern map[string]int
    LLMInsights      string // Unstructured insights from Gemini
}

func AnalyzeUser(ctx context.Context, userID string) (*UserAnalysis, error) {
    // 1. Pull all conversations
    conversations := db.GetUserConversations(userID)
    
    // 2. Build context for LLM
    prompt := buildAnalysisPrompt(conversations)
    
    // 3. Get insights
    insights, err := gemini.Analyze(ctx, prompt)
    
    return &UserAnalysis{
        UserID: userID,
        LLMInsights: insights,
        // ... structured metrics
    }, nil
}
```

### Phase 2: Minimal Event Tracking
```go
// internal/analytics/events.go
type UserAction struct {
    UserID   string
    Action   string // "download" | "share" | "credit_limit"
    Metadata map[string]interface{}
}

// Only track high-signal actions
func TrackDownload(userID, mediaID string) {
    db.Create(&UserAction{
        UserID: userID,
        Action: "download",
        Metadata: map[string]interface{}{"media_id": mediaID},
    })
}
```

### Phase 3: Daily Pipeline
```go
// cmd/analytics/daily.go
func RunDailyAnalysis() error {
    // 1. Get active users
    users := db.GetActiveUsersToday()
    
    // 2. Batch analyze (with rate limiting)
    for _, user := range users {
        analysis := AnalyzeUser(ctx, user.ID)
        StoreAnalysis(analysis)
        time.Sleep(100 * time.Millisecond) // Rate limit
    }
    
    // 3. Generate summary
    summary := GenerateDailySummary()
    SendToSlack(summary)
    
    return nil
}
```

## Scalability Strategy

**0-100 users**: 
- Manual review feasible
- Test analysis prompts
- Identify key patterns

**100-1,000 users**:
- Batch processing (nightly)
- Segment users by behavior
- Cache LLM insights

**1,000-10,000 users**:
- Sample-based analysis
- Cluster similar users
- Only deep-dive anomalies

**10,000+ users**:
- Statistical sampling
- Aggregate metrics
- LLM for outliers only

## Robustness

1. **Idempotent Processing**
   - Can re-run any day's analysis
   - No data corruption from failures

2. **Graceful Degradation**
   - If LLM fails, still have basic metrics
   - Manual SQL queries as backup

3. **Privacy First**
   - No PII in prompts
   - Aggregate insights only
   - User consent for deep analysis

## Success Metrics

**Week 1**: Can we identify why your sessions are successful?
**Week 2**: Can we predict which users will return?
**Week 4**: Can we identify the top 3 features to build?
**Week 8**: Can we segment users into clear personas?

## Migration Path

```sql
-- Start: Zero new tables
-- Week 1: Add one table
CREATE TABLE user_actions (
    id UUID PRIMARY KEY,
    user_id VARCHAR(255),
    action VARCHAR(50),
    metadata JSONB,
    created_at TIMESTAMP
);

-- Week 4: Add if needed
CREATE TABLE analysis_cache (
    user_id VARCHAR(255),
    analysis_date DATE,
    insights JSONB,
    PRIMARY KEY (user_id, analysis_date)
);
```

## Key Insight

Traditional analytics: "Track everything, hope to find patterns"
Our approach: "LLMs read conversations, we track only high-signal actions"

Result: 10x less code, 10x more insights.