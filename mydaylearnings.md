# My Daily Learnings - OpenClaw & AI Agent Development

> **Purpose:** Daily insights and learnings about OpenClaw, AI agent development, and building scalable systems as a solopreneur.
> 
> **Review Daily:** Read this file every day before starting any OpenClaw-related task to reinforce learnings and avoid repeating mistakes.

---

## Table of Contents

1. [OpenCl Wrapper Business Model](#openclaw-wrapper-business-model)
2. [OpenClaw Reality & Use Cases](#openclaw-reality--use-cases)
3. [Agent Memory Management](#agent-memory-management)
4. [Multi-Agent Orchestration](#multi-agent-orchestration)
5. [Key Principles for Success](#key-principles-for-success)

---

## OpenClaw Wrapper Business Model

### Core Insight

**The biggest opportunity isn't USING OpenClaw — it's SELLING it.**

OpenClaw is free and open-source. LLM costs are pennies. But setup requires knowledge, API configs, and custom skill writing. **95% of potential users will never do that themselves.**

That gap between power and accessibility... that's where money is.

### The Business Model

Configure OpenClaw for one specific use case, package it, sell to people who don't want to spend 10 hours setting it up themselves.

**Target buyers are NOT developers.** They are:
- Content creators
- Fitness coaches
- Agency owners
- Founders who want results without learning infrastructure

### Historical Pattern

Every major platform shift creates the same category of opportunity:
- WordPress created agencies
- Shopify created store builders
- App Store created indie developers
- **OpenClaw creates: AI agent wrapper sellers**

### Top 5 High-Potential Niches

#### 1. Content Creation Claw

**Value:** Flip the ratio from 80% production / 20% ideas to 80% ideas / 20% production.

**Features:**
- Web scraping skills monitoring X, Reddit, RSS feeds, YouTube transcripts
- Brand voice profile (JSON context file) shaping every output
- Content generation pipelines: Twitter posts, articles, newsletters, video scripts
- Image generation for thumbnails and social graphics
- Scheduling integrations across platforms

**Workflow:**
Agent runs on loop → scrapes sources → identifies trends → generates week of content in your voice → creates visuals → queues everything.

**User Experience:** Wake up to WhatsApp message: "Here's your content for the week. 14 posts, 2 newsletters, 3 video scripts. Review when ready."

#### 2. Fitness & Nutrition Claw

**Value:** Replaces MyFitnessPal + personal trainer + meal delivery service.

**Features:**
- Vision model integration for food recognition from photos (never manually log calories)
- Weekly meal plans based on goals and dietary preferences
- Grocery ordering through delivery APIs (literally does shopping for you)
- Apple Health sync (steps, sleep, heart rate, workout data)
- Main dashboard with all metrics in one place

**Killer Feature:** Identity Mode
- Choose coach personality: supportive mentor OR savage roaster
- Example: "Bro you walked 2,000 steps today and ordered dominos. Your Apple Watch is embarrassed to be on your wrist."
- **This makes it sticky.** People quit apps but don't quit their coach.

#### 3. Real-Life RPG Claw

**Target:** Gamers who can grind 12 hours but can't focus on real life goals for 30 minutes.

**Features:**
- Stats: Intelligence, Strength, Discipline, Social, Creativity
- XP system for completed tasks
- Daily quests optimized for current goals
- Boss fights (ship a feature by Friday)
- Character sheet dashboard with levels, streaks, achievements

**Psychology:** Gamification works because it turns abstract progress into visible numbers.
- You're not "getting healthier"
- You're "Level 14 Strength with 340 XP until next level"

**Viral Potential:** Users screenshot character sheets: "Just hit Level 20 Intelligence, 6 month streak." Free marketing.

#### 4. Coding Productivity Claw

**Value:** Pre-loaded context for every coding tool (Codex, Claude Code, Cursor).

**Features:**
- Sub-agents with optimized configs: system prompts, workspace settings, memory profiles
- Connected to best boilerplates and starter repos for every project type
- Pre-tuned for maximum output quality

**Workflow:**
1. Describe in plain English: "I need a SaaS dashboard with Stripe billing"
2. Agent pulls right boilerplate + configs
3. Spawns coding agent with everything ready
4. ONE-SHOTS almost all small to medium tasks

**Key Differentiator:** Everything is ready before you say a word.

#### 5. Business Operations Claw

**Target:** Solopreneurs needing to offload 15+ hours/week of routine work.

**Tasks:**
- Content/SEO pipelines: research → keywords → draft ads → queue for approval
- Financial ops: invoicing, follow-ups, payments, repetitive accounting
- Sales support: automated SDRs scraping leads, outbound outreach, qualification
- Customer support: personalized AI CSRs

---

## OpenClaw Reality & Use Cases

### Honest Assessment

**OpenClaw is real.** The value is real. But it's not for everyone.

**Best comparison:** OpenClaw is the **Linux of AI agents** — incredible power if you can configure it, frustrating overkill if you just want things to work.

### Who OpenClaw IS For

✅ **Solopreneurs** with digital products/services
✅ People who think "I wish I had a personal assistant who never sleeps"
✅ Those with way more to do than hours in the day
✅ Willing to invest 10+ hours in setup (seriously)
✅ Proficient enough with terminal, API keys, technical concepts

### Who OpenClaw is NOT For

❌ If ChatGPT/Claude/Grok/Gemini handles your needs fine
❌ If you're already a power user of Claude Code with MCP/n8n
❌ Companies with compliance requirements (enterprises: hard pass)
❌ People who want plug-and-play
❌ Those without time for the learning curve

### Top Practical Use Cases (Ranked by Value)

#### Tier 1: Personal Assistant Use Cases

**1. Email Automation / Inbox Zero**
- Value: Immediate and compounds over time
- Agent learns patterns, gets better at filtering
- Surfaces only what needs attention
- **Pro Tip:** Start in read-only mode for weeks before enabling autonomous actions

**2. Daily Briefing / Morning Digest**
- Universally recommended, easiest starting point
- 7AM message with everything important: weather, calendar, headlines, X trends, aggregated news
- Most missed feature if OpenClaw went away

**3. Personal Knowledge Base / Second Brain**
- Drop URLs, tweets, articles into chat
- Agent builds searchable memory via semantic search
- "Remember that article about the company that raised $50M for AI safety? What was their product?"
- Intelligence compounds with more info fed

**4. Automating Mundane Jobs**
- Example: Search for local electricians, contact all, get quotes, negotiate rates, schedule work
- Agents don't find things mundane or tedious
- **Real Example:** $4,200 saved on car negotiation
  - Scraped Reddit for pricing data
  - Pre-filled forms on 12 dealer websites
  - Played dealers against each other via competing quotes
  - Negotiated via email while owner did other things

**5. Health & Wellness**
- Food tracking via photo (vision model)
- Custom meditations & mental wellness
- Health reminders (stand up, walk, drink water)

#### Tier 2: Development Use Cases

**Value Shift:** From "useful" to "holy shit" — personal assistant to productive employee that ships code.

**Tasks:**
- Autonomous PR creation from bug reports
- Test suite monitoring with auto-fix on failure
- Deploy staging → production via Telegram message
- Cron-based health checks that actually DO something
- Multi-agent dev teams coordinated through chat

**Real-World Impact:**
- Monitoring forums/Reddit for bug fixes and enhancements
- Developing and shipping features overnight
- Solo devs: transformative
- Teams with mature CI/CD: additive, not game-changing

#### Tier 3: Business Operations

**Savings:** 15+ hours/week on routine business ops.

**Tasks:**
- Content/SEO pipelines (research → keywords → draft → queue)
- Financial ops (invoicing, follow-ups, payments)
- Sales support (lead scraping, outbound outreach, qualification)
- Customer support (personalized AI CSRs)

#### Tier 4: Emerging (High Potential)

**Financial Analysis**
- Crypto/stock screening (price, volume, momentum, EMA crosses)
- Buffett-style equity analysis (10Ks, earnings, transcripts)
- Crypto monitoring (wallets, volume movers)

**Market Research**
- Custom research reports on any topic
- X/Reddit screening for news and alerts
- Crypto whitepaper/tokenomics reviews

**WARNING:** "Autonomous trading" claims are exaggerated. Costs can get out of control with API bills.

### Where OpenClaw Really Shines

1. **As a personal assistant** — Designed to serve its master by remembering everything, becoming proactive, working 24/7
2. **For solopreneurs and technical power users** — Transformative
3. **When you can model-optimize** — Otherwise costs add up
4. **When you invest in setup** — Gap between power users and normal users is enormous

### Where OpenClaw Struggles

- **Janky system** — Gives fits at times
- **Security concerns** — Still worries
- **Not for teams/multi-player** — Not designed for it
- **High setup cost** — Requires 10+ hours to really benefit
- **Gap between demos and reality** — What power users achieve vs what normal people can setup

### Recommended Starting Path

1. **Daily briefing** — Low risk, low cost, immediate value, under 30 minutes to setup
2. **Second brain** — Feed all bookmarks, PDFs, links. Intelligence compounds with more you feed it
3. **Workflow automation** — Pick something specific to your day (dev, content, research, household) and automate it

### Future of OpenClaw

View it as POC for **agentic OS** — the next frontier for software.

Mainstream agentic products haven't been built yet, but everyone has noticed the buzz. Expect more normie-friendly products over time.

---

## Agent Memory Management

### The Core Problem

After long conversations, agents start losing earlier context. Not gradually — it just vanishes. Names, numbers, exact decisions gone.

**Root Cause:** Compaction

When context window fills up, OpenClaw compresses older messages into a summary. Summary captures gist but drops specifics.

Default behavior treats everything equally — your carefully crafted instruction from message #3 gets same treatment as casual small talk from message #7.

### Solution 1: Memory Flush Before Compaction

**Purpose:** Write important context to disk BEFORE compressor runs.

**Config:**
```json
{
  "compaction": {
    "memoryFlush": {
      "enabled": true,
      "softThresholdTokens": 4000
    }
  }
}
```

**How it works:**
- Session approaches context limit
- Silent turn reminds agent to save durable facts to `memory/YYYY-MM-DD.md`
- Agent writes what matters
- Compaction runs
- Important stuff survives on disk even if context summary loses it

**Key Learning:** Compaction is not your enemy. Losing information during compaction is. Make sure anything worth remembering gets written to a file before compressor touches it.

### Solution 2: Hybrid Search (QMD)

**Problem:** Pure semantic search works for broad queries but struggles with:
- Proper nouns
- Specific numbers
- Exact phrases

**Example:** Search for "Charles payment failure" → returns results about completely different topic that used similar language.

**Solution:** QMD combines BM25 (keyword matching) + vector embeddings + reranker.

When you search, it finds results that:
- Contain exact words (BM25)
- Are semantically related (vectors)
- Then reranks by relevance

**Config:**
```json
{
  "memory": {
    "qmd": {
      "paths": [
        {
          "path": "/Users/ramya/clawd",
          "name": "memory-root",
          "pattern": "MEMORY.md"
        },
        {
          "path": "/Users/ramya/clawd/memory",
          "name": "memory-dir",
          "pattern": "**/*.md"
        },
        {
          "path": "/Users/ramya/clawd/learnings",
          "name": "learnings",
          "pattern": "**/*.md"
        }
      ]
    }
  }
}
```

**Key Learning:** There's a difference between "information exists" and "agent uses information." Search infrastructure handles the first part. Boot instructions and retrieval habits handle the second. Test both separately.

### Solution 3: Explicit Retrieval Instructions

**Problem:** Retrieval is not automatic. Agent has to decide to search. If conversation doesn't trigger right cues, it won't look things up.

**Solution:** Add explicit retrieval instructions to boot sequence:

```markdown
Before starting any task:
- Search daily logs for related context
- Check for rules about this type of task
- If a client is mentioned, search for their history
```

**Testing Protocol:** Plant a marker in daily log:
```markdown
MARKER: 2026-02-20 — Remember to always check git status before claiming code is pushed.
```

Start new session, ask: "What was the marker from yesterday?" If found → retrieval working. If not → broken.

### Solution 4: Aggressive Context Pruning

**Problem:** Long sessions hit compaction multiple times. Memory flush only triggers once per cycle. After that, everything's at risk.

**Solution:** Context pruning works alongside compaction:

```json
{
  "contextPruning": {
    "mode": "cache-ttl",
    "ttl": "6h",
    "keepLastAssistants": 3
  }
}
```

**How it works:**
- Aggressively prunes old context after 6 hours
- Keeps last 3 assistant responses
- Combined with memory flush → important stuff written to disk early
- Old context cleaned up before overflow

**Testing:** After any significant config change, plant marker and test retrieval across compaction boundaries. If marker survives → change worked. If not → broken.

### Solution 5: Boot Sequence Cleanup

**Problem:** Agent was loading 11,887 tokens of system prompt before reading user message:
- 51 skills (20 never used)
- 200 lines of company wiki loaded every session
- Two competing boot sequences (one OpenClaw doesn't even recognize)
- Every model switch → complete memory wipe, no handover protocol

**Root Cause:** OpenClaw auto-reads these files on every new session:
- `SOUL.md`, `USER.md`, `MEMORY.md`, `AGENTS.md`, `TOOLS.md`
- Everything else (daily logs, docs, reference files) → agent must read itself

**Solution:** Full audit and cleanup:

1. Move boot sequence to TOP of `SOUL.md` (only reliable place)
2. Delete `IDENTITY.md` (not recognized by OpenClaw)
3. Delete `BOOTSTRAP.md` (one-time onboarding, wasting 361 tokens/session)
4. Slim `AGENTS.md` from 200 lines to 90 (move docs to docs/ folder)
5. Remove 20 unused marketing skills (eating 3,000 tokens/session)

### Memory Management Checklist

- [ ] Memory flush enabled before compaction
- [ ] Hybrid search (QMD) configured
- [ ] Explicit retrieval instructions in boot sequence
- [ ] Context pruning enabled (6h TTL)
- [ ] Boot sequence audited and cleaned
- [ ] Testing protocol implemented (markers + retrieval verification)
- [ ] Test memory system under load (long 2+ hour sessions)

---

## Multi-Agent Orchestration

### The Core Idea

**Don't use Codex or Claude Code directly. Use OpenClaw as orchestration layer.**

Your orchestrator spawns agents, writes their prompts, picks the right model for each task, monitors progress, and notifies you when work is complete.

### Why Orchestration Matters

**Context windows are zero-sum.** You have to choose what goes in.

- Fill it with code → no room for business context
- Fill with customer history → no room for codebase

**Two-tier system works:** Each AI is loaded with exactly what it needs.

### Proof Points (Last 4 Weeks)

- **94 commits in one day** — most productive day, had 3 client calls, didn't open editor once
- **Average: ~50 commits/day**
- **7 PRs in 30 minutes** — idea to production blazing fast (coding + validations automated)
- **Commits → MRR** — Bundling with founder-led sales to deliver feature requests same-day

### System Architecture

```
Orchestrator (OpenClaw)
    ├─ Context: Business context (customer data, meetings, decisions)
    ├─ Memory: Obsidian vault with historical context
    ├─ Agents:
    │   ├─ Codex (specific coding tasks)
    │   ├─ Claude Code (code reviews)
    │   └─ Gemini (security reviews)
    └─ Monitoring: Cron job checking status every 10 minutes
```

### Specialization Through Context, Not Models

OpenClaw and Codex have drastically different context:

**OpenClaw:** High strategy, business context, orchestration
**Codex/Claude:** Code-focused, specific technical prompts

Each agent gets exactly what it needs, nothing more.

### Real Example: Customer Request → Scoping → Spawn Agent → Monitor → Deliver

#### Step 1: Customer Call → Scoping with Orchestrator

After agency call:
- Talk through request with orchestrator
- Meeting notes sync automatically to Obsidian vault
- Zero explanation needed
- Scope out feature together
- Land on template system for saving configs

**Orchestrator then does:**
1. Tops up credits to unblock customer (admin API access)
2. Pulls customer config from prod DB (read-only access)
3. Spawns Codex agent with detailed prompt containing all context

#### Step 2: Spawn Agent with Isolated Worktree

```bash
# Create worktree + spawn agent
git worktree add ../feat-custom-templates -b feat/custom-templates origin/main
cd ../feat-custom-templates && pnpm install

tmux new-session -d -s "codex-templates" \
  -c "/Users/elvis/Documents/GitHub/medialyst-worktrees/feat-custom-templates" \
  "$HOME/.codex-agent/run-agent.sh templates gpt-5.3-codex high"
```

**Why tmux?**
- Agent going wrong direction? Don't kill it, send keys with context
- "The schema is in src/types/template.ts. Use that." → mid-task guidance

#### Step 3: Track Task in Registry

```json
{
  "id": "feat-custom-templates",
  "tmuxSession": "codex-templates",
  "agent": "codex",
  "description": "Custom email templates for agency customer",
  "repo": "medialyst",
  "worktree": "feat-custom-templates",
  "branch": "feat/custom-templates",
  "startedAt": 1740268800000,
  "status": "running",
  "notifyOnComplete": true
}
```

#### Step 4: Monitor Loop

Cron job every 10 minutes:
```bash
# .clawdbot/check-agents.sh
- Checks if tmux sessions are alive
- Checks for open PRs on tracked branches
- Checks CI status via gh cli
- Auto-respawns failed agents (max 3 attempts)
- Only alerts if something needs human attention
```

**Key:** Not watching terminals directly. System tells you when to look.

#### Step 5: Agent Creates PR

Definition of done (agent knows this):
- PR created
- Branch synced to main (no merge conflicts)
- CI passing (lint, types, unit tests, E2E)
- Codex review passed
- Claude Code review passed
- Gemini review passed
- Screenshots included (if UI changes)

#### Step 6: Automated Code Review (3 Agents)

**Codex Reviewer:**
- Exceptional at edge cases
- Most thorough review
- Catches logic errors, missing error handling, race conditions
- False positive rate very low

**Gemini Code Assist Reviewer:**
- Free and incredibly useful
- Catches security issues, scalability problems
- Suggests specific fixes
- No-brainer to install

**Claude Code Reviewer:**
- Mostly useless — overly cautious
- "Consider adding..." suggestions usually overengineering
- Skip everything unless marked critical
- Validates what other reviewers flag

All three post comments directly on PR.

#### Step 7: Automated Testing

CI pipeline runs:
- Lint and TypeScript checks
- Unit tests
- E2E tests (Playwright against preview environment)
- **New rule:** If PR changes any UI, must include screenshot in PR description

### Launching Agents

```bash
# Codex
codex --model gpt-5.3-codex \
  -c "model_reasoning_effort=high" \
  --dangerously-bypass-approvals-and-sandbox \
  "Your prompt here"

# Claude Code
claude --model claude-opus-4.5 \
  --dangerously-skip-permissions \
  -p "Your prompt here"
```

**Why tmux over direct exec?**
- Mid-task redirection is powerful
- Don't need to kill and restart
- Send context when agent goes wrong direction

### Orchestration Checklist

- [ ] Orchestrator (Zoe) configured with business context
- [ ] Agent spawning via isolated worktrees
- [ ] Task tracking in JSON registry
- [ ] Monitoring loop (cron every 10 minutes)
- [ ] Auto-respawn on failure (max 3 attempts)
- [ ] Automated code review (3 agents)
- [ ] Automated testing pipeline
- [ ] Definition of "done" clear to agents
- [ ] PR workflow: create → review → test → notify
- [ ] Cost tracking: ~$100/month Claude + $90/month Codex

---

## Key Principles for Success

### For Building OpenClaw Wrappers

1. **Solve ONE problem really well** — Don't try to be everything
2. **Pre-configure EVERYTHING** — Your value is in setup, not usage
3. **Identity matters** — Give your wrapper personality (coach, mentor, teammate)
4. **Test across use cases** — Real users will use it differently than you
5. **Documentation is key** — If you can't explain it, you can't sell it
6. **Price for outcomes** — Sell time saved, revenue gained, problems solved

### For OpenClaw Power Users

1. **Memory management is critical** — Set up flush, hybrid search, retrieval instructions
2. **Boot sequence audit** — Clean SOUL.md, remove unused skills, optimize token usage
3. **Test under load** — Long sessions are where memory systems break
4. **Context is zero-sum** — Choose what goes in: business OR code, not both
5. **Orchestration beats everything** — One OpenClaw managing multiple specialized agents

### For Multi-Agent Development

1. **Isolation is key** — Each agent in own worktree/tmux session
2. **Monitor, don't babysit** — Automated checks tell you when to look
3. **Define "done" explicitly** — Agent must know what completion looks like
4. **Multiple reviewers, not just one** — Each model catches different things
5. **Screenshots for UI changes** — Dramatically shortens review time
6. **One-shots are possible** — With proper context, almost all small-medium tasks complete without intervention

### General Development Wisdom

1. **Compaction is not the enemy** — Losing info during compaction is
2. **Search vs retrieval** — Both are required. Infrastructure handles first, instructions handle second
3. **Hybrid search > pure semantic** — Keywords + vectors + reranking
4. **Boot sequence matters** — Put important instructions at TOP of SOUL.md
5. **Test memory, don't trust it** — Plant markers, verify retrieval
6. **Long sessions reveal bugs** — Test memory system under real load, not quick chats
7. **Context window is finite** — Assume everything in conversation is temporary unless written to disk
8. **Token efficiency compounds** — 3,000 tokens/session saved = massive savings over time

---

## Daily Review Checklist

Before starting any OpenClaw-related task, review:

### Memory System
- [ ] Memory flush enabled
- [ ] Hybrid search (QMD) configured
- [ ] Boot sequence optimized
- [ ] Retrieval instructions explicit
- [ ] Testing protocol in place

### Agent Orchestration
- [ ] Orchestrator configured with business context
- [ ] Agent spawning via worktrees
- [ ] Task tracking in place
- [ ] Monitoring loop active
- [ ] Definition of "done" clear

### General
- [ ] SOUL.md clean and optimized
- [ ] All files auto-loaded by OpenClaw are correct
- [ ] Unused skills removed
- [ ] Token usage audited
- [ ] No redundant instructions

---

**Last Updated:** 2026-02-24
**Next Review Date:** 2026-02-25

> *This document is a living guide. Update it as you learn more. The goal is to compound intelligence over time.*
