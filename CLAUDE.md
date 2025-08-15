# CLAUDE.md - AI Assistant Guide for Latent Product Development

> Your guide for working with Claude on the Latent platform

## 🎯 Purpose

This document helps Claude (and other AI assistants) quickly understand the Latent platform architecture, make informed decisions, and contribute effectively to daily product iterations.

## 🚀 Quick Start for Claude

When starting a new session, always:

1. **START HERE**: `/Users/admin/Desktop/latent-product/UX-TODO.md` - Current priorities and what to build
2. **Check this directory**: `/Users/admin/Desktop/latent-product/`
3. **Review if needed**:
   - `CONTRIBUTING.md` - Code standards and workflows
   - `brand-marketer-use-case.md` - Target user deep-dive
   - User Stories in platform-api-v2/docs/user-stories/
3. **Understand the codebase locations**:
   - `/Users/admin/Desktop/latent-ui` - Frontend (React)
   - `/Users/admin/Desktop/latent-platform-api-v2` - Orchestration API (Go)
   - `/Users/admin/Desktop/latent-video-ai-v2` - AI services (Go)
   - `/Users/admin/Desktop/oauth-service` - Authentication (Node.js)

## 🏗️ Platform Overview

### Core Vision
"Where consciousness meets creation" - Latent transforms creative ideas into cinematic narratives through an ethereal, minimalist interface.

### Architecture Summary
```
Frontend (React) → OAuth (Google) → Platform API (Orchestration) → AI Services (LLM)
                                          ↓
                                    PostgreSQL + Redis
```

### Key Technologies
- **Frontend**: React, Tailwind CSS, Framer Motion, Zustand
- **Backend**: Go, Gin, Node.js, Express
- **Database**: PostgreSQL, Redis
- **AI**: OpenAI/Anthropic APIs
- **Auth**: Google OAuth 2.0, JWT

## 📋 Common Tasks & Patterns

### Adding a New Feature

1. **Frontend Route**
   ```jsx
   // In latent-ui/src/App.jsx
   <Route path="/new-feature" element={
     <ProtectedRoute>
       <NewFeaturePage />
     </ProtectedRoute>
   } />
   ```

2. **API Endpoint**
   ```go
   // In platform-api-v2
   router.POST("/api/feature", authMiddleware, handleFeature)
   ```

3. **AI Service**
   ```go
   // In video-ai-v2
   type FeatureService struct {
       llm LLMClient
   }
   ```

### UI Component Pattern
```jsx
// Pages are route handlers
const FeaturePage = () => {
  const { isAuthed } = useAuth();
  return <FeatureFlow />;
};

// Organisms handle logic
const FeatureFlow = () => {
  const [data, setData] = useState();
  // Fetch data, manage state
  return <FeatureScreen data={data} />;
};

// Screens are props-only
const FeatureScreen = ({ data, onAction }) => {
  // Pure presentation
  return <div>...</div>;
};
```

### API Pattern
```go
// Handler
func (h *Handler) CreateFeature(c *gin.Context) {
    var req CreateFeatureRequest
    if err := c.ShouldBindJSON(&req); err != nil {
        c.JSON(400, gin.H{"error": "invalid request"})
        return
    }
    
    result, err := h.service.CreateFeature(c.Request.Context(), req)
    if err != nil {
        c.JSON(500, gin.H{"error": err.Error()})
        return
    }
    
    c.JSON(200, result)
}
```

## 🎨 Design Philosophy

### Core Principles
1. **Minimalism as Poetry** - Remove everything that doesn't serve the story
2. **Ethereal Interactions** - Blur transitions, consciousness-like flows
3. **Typography First** - Words are the primary interface element
4. **One Thing at a Time** - Never overwhelm, always focus

### UI Guidelines
- **Colors**: Black background, white text, subtle glass effects
- **Spacing**: Generous, let the interface breathe
- **Animations**: 0.6s duration, blur(10px) transitions
- **Components**: Atoms → Molecules → Organisms → Pages

## 🔧 Development Workflow

### Daily Iteration Cycle
```
User Feedback → Product Decision → Claude Implementation → Test → Deploy
     ↑                                                              ↓
     └──────────────────────────────────────────────────────────────┘
```

### For New Features
1. **Start with user need** - What problem are we solving?
2. **Design the experience** - How does it feel to use?
3. **Implement incrementally** - Small, tested changes
4. **Gather feedback** - Deploy and learn

### For Bug Fixes
1. **Reproduce locally** - Understand the issue
2. **Write a test** - Prevent regression
3. **Fix minimally** - Don't refactor unnecessarily
4. **Verify fix** - Test the specific scenario

## 🚨 Important Context

### Current Challenges
1. **Auth Complexity** - Multiple services need to be running
2. **Development Setup** - Takes time to get all services running
3. **No Mock Mode** - Can't easily bypass auth for prototyping

### Technical Debt
- Frontend and backend are separate repos (considering monorepo)
- Some API endpoints documented only in test scripts
- Limited automated testing

### Performance Considerations
- Platform API can become bottleneck during multi-service calls
- Redis needed for job queue management
- LLM API rate limits affect throughput

## 🛠️ Debugging Guide

### Common Issues

**"Awakening your creative space..." stuck**
- OAuth service not running (port 3000)
- Check browser console for auth errors

**API Connection Errors**
```bash
# Check all services
curl http://localhost:3000/health     # OAuth
curl http://localhost:8085/health    # Platform API
curl http://localhost:8084/health    # Video AI
```

**Style Issues**
```bash
# Clear Vite cache
cd latent-ui
rm -rf node_modules/.vite
npm run dev
```

## 📝 Code Review Checklist

When reviewing or writing code, ensure:

- [ ] Follows design philosophy (ethereal, minimal)
- [ ] Maintains existing patterns
- [ ] Handles errors gracefully
- [ ] Includes necessary documentation
- [ ] Preserves the "time disappears" experience
- [ ] No console.logs left in production code
- [ ] API changes are backward compatible

## 🎯 Product Priorities

### Current Focus
1. **Personality-driven video generation** - URL → Context → Script → Video
2. **Creative Memory** - Making the system learn from user creations
3. **Seamless experience** - Removing friction from the creative flow

### Future Vision
- Multi-modal creation (images, music, 3D)
- Collaborative creation spaces
- API platform for developers
- Mobile-first experience

## 💡 Tips for Claude Sessions

### DO:
- Read existing code before writing new code
- Follow established patterns religiously
- Ask clarifying questions when requirements are vague
- Suggest improvements while respecting the vision
- Test changes before confirming completion
- Keep responses concise and actionable

### DON'T:
- Add unnecessary complexity
- Break existing functionality
- Ignore the design philosophy
- Create new patterns without discussion
- Add dependencies without justification
- Generate verbose explanations when code speaks clearly

## 🔗 Quick References

### Service Ports
- Frontend: `http://localhost:5173`
- OAuth: `http://localhost:3000`
- Platform API: `http://localhost:8085`
- Video AI: `http://localhost:8084`

### Key API Endpoints
```bash
# Create latent
POST /api/latents

# Multi-service generation
POST /api/v2/generate

# Check request status
GET /api/v2/requests/:id

# Real-time updates
GET /api/events (SSE)
```

### Environment Variables
```bash
# Frontend (.env.local)
VITE_OAUTH_API_URL=http://localhost:3000
VITE_PLATFORM_API_URL=http://localhost:8085

# Platform API (.env)
DATABASE_URL=postgresql://...
REDIS_URL=redis://...
VIDEO_AI_URL=http://localhost:8084
```

## 🚀 Starting a Claude Session

**Optimal First Message:**
```
Hey Claude, I need help with [specific task].

Context:
- Working on Latent platform (consciousness meets creation)
- Docs are in /Users/admin/Desktop/latent-product/
- Code is in /Users/admin/Desktop/[service-name]/
- [Any specific requirements or constraints]

Please start by reviewing the relevant documentation, then help me implement [specific feature/fix].
```

## 📊 Success Metrics

When implementing features, optimize for:
1. **Time to First Value** - How quickly can users create?
2. **Completion Rate** - Do users finish their creations?
3. **Return Rate** - Do users come back?
4. **Share Rate** - Do users share their creations?

## 🎬 Final Thoughts

Remember: We're not building a video editor. We're crafting a space where human consciousness can express itself through moving images. Every line of code should serve this vision.

Make it feel like thoughts becoming reality.

---

*Last Updated: January 2025*
*Version: 1.0*