# UX TODO

## 🎯 Target Users
- **Creative introspectors** - Artists & creators seeking self-discovery through their creative patterns, using art as a mirror for consciousness and personal growth. These users create emotional dependency, not just functional utility. They're not using Latent to make things; they're using it to understand who they are. When someone discovers their depression lifting through progressively brighter color palettes in their creations, that's not a tool anymore - it's a companion to their inner life.
- **Creative artists** - Artists who sell work on Dribbble for tokens/NFTs in crypto ecosystem. Requires insanely contextual and personalized media
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
2. [ ] **Strengthen User-Level Context** - Ensure tool results incorporate user context for personalized outputs
3. [ ] **Daily Heartbeat Integrations** - Connect with fitness devices (Garmin, Whoop), GitHub, and other daily data sources
4. [ ] **Latent Native Media Model** - WebGL, programmatic creation based on Latent design theme
5. [ ] **User Story #001: Sunset Family Video** - End-to-end photo+scene→video workflow
6. [ ] **Chat sharing** - Public links for virality
7. [ ] **Veo3 Video Generation** - Google's latest video model integration
8. [ ] **Open Source Models Integration** - Alternative generation options (Qwen, WAN 2.2)
9. [ ] **Max Edit Kontext** - Advanced editing capabilities
10. [ ] **Slide Deck Generation** - Programmatic slide creation
11. [ ] **Command & control interface** - Power user features
12. [ ] **Chat naming** - Auto-generate from content

### 🔧 Infrastructure (Backend & Operations)
1. [ ] **Basic Analytics Queries** - Understand what users want (TODAY)
2. [ ] **Analytics Table for Tool Results** - Separate table for analytics on tool results
3. [ ] **Platform API v2 Events Schema** - Fixed events schema (now v2)
4. [ ] **Credits + Stripe Integration** - Stop bleeding money
5. [ ] **Credits Check Before Tool Execution** - Enforce limits before running tools
6. [ ] **GCP Direct Deployment** - Production infrastructure on Google Cloud
7. [ ] **Production Deployment Pipeline** - CI/CD with GitHub Actions
8. [ ] **Monitoring & Operations** - Dashboards, alerts, logging
9. [ ] **Remove legacy code in video-ai-v2** - Clean up unused/deprecated code
10. [ ] **Clean up UI repo** - Tech debt







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
- **Context Service**: `/Users/admin/Desktop/latent-context-service` - LLM-powered user insights (port 8088)


**Process:**
1. TODO defines what users need
2. User stories detail the experience
3. Build generic infrastructure
4. Ship, measure, refine TODO

