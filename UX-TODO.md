# UX TODO

## 🎯 Target Users
- **Creative introspectors** - Artists & creators seeking self-discovery through their creative patterns, using art as a mirror for consciousness and personal growth. These users create emotional dependency, not just functional utility. They're not using Latent to make things; they're using it to understand who they are. When someone discovers their depression lifting through progressively brighter color palettes in their creations, that's not a tool anymore - it's a companion to their inner life.
- **Early-stage consumer founders** (Singapore, SEA, India) - Need rapid content iteration
- **Content creators** - Individual creators & influencers needing daily content

## 📊 User Funnel & Key Personas
- **Mallika**: "What should I be marketing to?"
- **Brian Blum**: Keywords targeting strategy
- **Borar, Rishi**: Early-stage founders needing rapid content

### Awareness Generation

#### 1. **Daily Ghibli-style TikToks** (Start Immediately)
Post one 15-30 second animated short daily:
- Monday: "What your coffee order says about your soul" (whimsical animation)
- Tuesday: "The ghost in your notifications" (digital loneliness) 
- Wednesday: "Your inner child's last text to you" (nostalgia)
- Thursday: "The color of your Tuesday at 3pm" (mundane magic)
- Friday: "Your weekend's opening credits" (anticipation)

Each ends with: "Created in 47 seconds on Latent"

**Why this works**: Ghibli aesthetic = instant emotional connection + shareability. You're not selling AI tools, you're selling feelings.

#### 2. **The Mirror Campaign** (Week 2)
Launch after basic analytics are ready:
- Partner with 5 micro-influencers in therapy/self-help space
- Give them free access for 7 days
- Day 8: They post their "Latent Mirror" revealing their unconscious patterns
- Hook: "This AI read my art and told me I was grieving before I knew it myself"

### The Share: "My Latent Mirror"
**Viral hook for creative introspectors - shareable after 2-4 days**

- **Format**: Vertical mobile screenshot (Instagram story-like)
- **Structure**:
  - Black background with "Day 3 on Latent" at top
  - Generated image center (e.g., figure in doorway, half shadow/half light)
  - Pattern reveal: **"doorway"** *You've used this word 7 times since Tuesday*
  - Glass-morphism card showing unconscious patterns:
    - Transitions, thresholds, liminal spaces
    - Actual prompts: "between jobs", "looking for an exit", "new chapter"
  - Bottom insight: *"You're not creating art. You're creating maps of where you're going."*
  - CTA: "See your patterns → latent.ai/mirror"
- **Why it spreads**: Names what users couldn't articulate about themselves
- **Hook**: "Within 48 hours, Latent knew me better than I knew myself"

## 🚨 P0 - Ship Blockers

### 🎨 UX (User-Facing Features)
1. [ ] **My Latent Mirror Implementation** - Build shareable self-discovery feature (VIRAL HOOK)
2. [ ] **User Story #001: Sunset Family Video** - End-to-end photo+scene→video workflow
3. [ ] **Memory View (/memory)** - Self-reflection through creative history
4. [ ] **User-level memory** - Remember preferences, style, past creations
5. [ ] **Veo3 Video Generation** - Google's latest video model integration
6. [ ] **Open Source Models Integration** - Alternative generation options (Qwen, WAN 2.2)
7. [ ] **Chat sharing** - Public links for virality
8. [ ] **Intelligent tool calling** - Context-aware tool selection
9. [ ] **Conversation context** - Maintain context across messages
10. [ ] **Chat naming** - Auto-generate from content
11. [ ] **Improve design for handling multiple tool result components** - Better UI for complex tool chains
12. [ ] **Slide Deck Generation** - Programmatic slide creation
13. [ ] **"Raise Request" UI** - Button/modal for users to submit feature requests
14. [ ] **Script editing in chat** - Canvas mode
15. [ ] **Command & control interface** - Power user features
16. [ ] **Flux Kontext integration** - Product placement & compositing
17. [ ] **Max Edit Kontext** - Advanced editing capabilities
18. [ ] **Ideogram character edit** - Character consistency

### 🔧 Infrastructure (Backend & Operations)
1. [ ] **Basic Analytics Queries** - Understand what users want (TODAY)
2. [ ] **GCP Direct Deployment** - Production infrastructure on Google Cloud
3. [ ] **Credits + Stripe Integration** - Stop bleeding money
4. [ ] **Credits Check Before Tool Execution** - Enforce limits before running tools
5. [ ] **Chat Analytics Dashboard** - Understand user behavior
6. [ ] **Automated Insights Pipeline** - LLM analyzes chat data weekly
7. [ ] **Business Model Decision** - Implement tiers or micropayments
8. [ ] **Service Configuration** - Dockerize, auth, VPC, secrets
9. [ ] **DNS & Load Balancing** - latentshot.com configuration
10. [ ] **Production Deployment Pipeline** - CI/CD with GitHub Actions
11. [ ] **Monitoring & Operations** - Dashboards, alerts, logging
12. [ ] **Request aggregation pipeline** - Collect and store user feedback
13. [ ] **LLM-powered prioritization** - AI analyzes requests and auto-updates this TODO
14. [ ] **Remove legacy code in video-ai-v2** - Clean up unused/deprecated code
15. [ ] **Clean up UI repo** - Tech debt
16. [ ] **Ensure type safety in platform API v2** - Type-safe Go code


## 📝 Notes
- Challenge: Product placement requires Flux Multi or LORAs
- Need parity with competitor features
- Focus on network effects for growth

## 🔮 Vision: User-Driven Development
**Raise Request System**: Users submit feature requests → LLM analyzes patterns → Auto-updates this TODO
- Democratizes product development
- Ensures we build what users actually want
- Creates tight feedback loop
- Scales product management with AI

## 📖 Working Backwards Framework
**Everything flows from this document:**
```
UX-TODO.md (North Star)
    ↓
User Stories (Platform API: /docs/user-stories/)
    ↓
Technical Implementation
    ↓
Ship & Learn → Update TODO
```

### 🗂️ Repository Map
- **UI/Frontend**: `/Users/admin/Desktop/latent-ui` - React, chat interface, user interactions
- **Chat AI**: `/Users/admin/Desktop/latent-chat-ai` - Agent orchestration, tool calling, Gemini integration
- **Platform API**: `/Users/admin/Desktop/latent-platform-api-v2` - Orchestration, tool registry, database, events
- **Video AI**: `/Users/admin/Desktop/latent-video-ai-v2` - Image/video generation (Imagen, Veo, Flux)
- **Auth Service**: `/Users/admin/Desktop/oauth-service` - Google OAuth, JWT tokens

**Active User Stories:**
- [001: Sunset Family Video](../latent-platform-api-v2/docs/user-stories/001-sunset-family-video.md) - Photo + Scene → Video
- 002: Recipe to Cooking Video (Planned)
- 003: Podcast Clip Generator (Planned)

**Process:**
1. TODO defines what users need
2. User stories detail the experience
3. Build generic infrastructure
4. Ship, measure, refine TODO

