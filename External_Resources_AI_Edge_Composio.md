# External Resources - AI Edge Computing, Agent Orchestration & Best Practices

> **Purpose:** Curated collection of external resources from X, GitHub, and YouTube for building $1B solopreneur company
> **Source:** Links provided by user, fetched from GitHub and external sources
> **Last Updated:** 2026-02-24

---

## Table of Contents

1. [Agent Orchestrator - ComposioHQ](#agent-orchestrator---composiohq)
2. [Key Insights from X Posts](#key-insights-from-x-posts)
3. [Best Practices](#best-practices)
4. [Additional Resources](#additional-resources)

---

## Agent Orchestrator - ComposioHQ

### Repository Overview

**Repository:** https://github.com/ComposioHQ/agent-orchestrator

### What It Is

**"Agentic orchestrator for parallel coding agents — each in its own git worktree. Agents autonomously fix CI failures, address review comments, and open PRs. You supervise from one dashboard."**

### Core Features

1. **Parallel Agent Management**
   - Each agent gets its own isolated git worktree
   - Each agent gets its own branch
   - Each agent gets its own PRs
   - Auto-creation of PRs

2. **Autonomous CI/CD**
   - Agents automatically detect and fix CI failures
   - No manual intervention required
   - Seamless integration with GitHub Actions, etc.

3. **Code Review Management**
   - Agents automatically address review comments
   - Agents automatically make requested changes
   - You only get pulled in when human judgment is needed

4. **Merge Conflict Resolution**
   - Agents automatically handle merge conflicts
   - Auto-rebase when necessary
   - Maintains clean commit history

5. **Dashboard Supervision**
   - Web dashboard at http://localhost:3000
   - Overview of all sessions
   - Real-time agent status
   - Control from one interface

### Installation & Setup

#### Step 1: Clone and Install

```bash
# Clone the repository
git clone https://github.com/ComposioHQ/agent-orchestrator.git
cd agent-orchestrator

# Install dependencies
npm install

# Set up your project
cd ~/your-project
ao init --auto
```

#### Step 2: Configure

```yaml
# agent-orchestrator.yaml
port: 3000

defaults:
  runtime: tmux  # Terminal multiplexer
  agent: claude-code  # Agent model (claude-code, codex, or aider)
  workspace: worktree  # Isolated git worktree

projects:
  my-app:
    repo: owner/my-app
    path: ~/my-app
    defaultBranch: main
    sessionPrefix: app

reactions:
  ci-failed:
    auto: true
    action: send-to-agent
    retries: 2
  changes-requested:
    auto: true
  escalateAfter: 30m
  approved-and-green:
    auto: false  # Flip to true for auto-merge

notifiers:
  - desktop
```

### Key Commands

```bash
# Launch and spawn an agent
ao start
ao spawn my-project 123  # GitHub issue, Linear ticket, or ad-hoc

# Session management
ao session ls    # List sessions
ao session kill <session>    # Kill a session
ao session restore <session>  # Revive a crashed agent
ao status              # Overview of all sessions
ao spawn <project>     # Send instructions to agent

# Dashboard
ao dashboard  # Open web dashboard
```

### Architecture

```
┌─────────────────────────────────────────────┐
│            YOU (Human Architect)         │
│                                         │
│    ┌─────────────────────────┐       │
│    │  AGENT ORCHESTRATOR  │       │
│    │  (ao)                │       │
│    │                        │       │
│    │  ┌──────────────────┐  │       │
│    │  │  AGENT 1          │  │       │
│    │  ├─ worktree          │  │       │
│    │  ├─ branch            │  │       │
│    │  └─ PRs              │  │       │
│    └──────────────────────────┘  │       │
│    │  ┌──────────────────┐  │       │
│    │  │  AGENT 2          │  │       │
│    │  ├─ worktree          │  │       │
│    │  ├─ branch            │  │       │
│    │  └─ PRs              │  │       │
│    └──────────────────────────┘  │       │
│                                         │
│    ┌─────────────────────────┐       │
│    │  WEB DASHBOARD        │       │
│    └─ http://localhost:3000 │       │
│                                         │
└─────────────────────────────────────────────┘
```

### Technology Stack

- **Node.js** 20+
- **Git** 2.25+
- **tmux** (default) or **Docker**, **k8s**, **process** (alternate runtimes)
- **Agents:** claude-code (default), codex, aider (alternatives)
- **Trackers:** GitHub (default), Linear (alternative)
- **Notifiers:** desktop (default), Slack, composio, webhook (alternatives)
- **SCM:** GitHub (default)

### Key Features from Documentation

**Reactions:**
- **CI fails** → agent gets logs and fixes it
- **Reviewers request changes** → agent addresses them
- **PR approved with green CI** → you get notification

**Project Structure:**
- Each project has isolated git worktree
- Each project has defaultBranch and sessionPrefix
- Workspace is absolute path (~/projects/app-name)
- Auto-creation of feature branches

**Runtime-agnostic:**
- tmux: Default, works on any Unix system
- Docker: Containerized agents
- k8s: Production orchestration
- process: For simple setups

### Best Practices from ComposioHQ

1. **Start Simple**
   - Use default configuration
   - Start with one project
   - Add complexity as needed

2. **Define Clear Boundaries**
   - Agents should know what to do autonomously
   - Define when to escalate to human
   - Set up approval workflows

3. **Monitor Before Scaling**
   - Run a single agent successfully
   - Understand the workflow
   - Then add parallel agents

4. **Use Worktrees for Isolation**
   - Each agent works in its own tree
   - No conflicts between agents
   - Clean git history per project

5. **Review PRs Promptly**
   - Don't let PRs pile up
   - Review and merge quickly
   - Agents handle most changes automatically

6. **Set Up Notifications**
   - Configure notifiers for important events
   - Use appropriate severity levels
   - Set up escalation rules

### Examples from Repository

**Scenario 1: CI Failure**
```
# Problem: CI fails
# Without orchestrator: Manually check logs, restart, fix
# With orchestrator:
ao status  # Check which agent is affected
# Agent automatically:
# - Gets CI logs
# - Analyzes failure
# - Fixes the issue
# - Retries the job
# - Notifies you only on persistent failures
```

**Scenario 2: Code Review**
```
# Problem: Reviewer leaves comments
# Without orchestrator: Manually address each comment
# With orchestrator:
# Agent automatically:
# - Reads review comments
# - Understands feedback
# - Makes requested changes
# - Pushes updates to PR
# - Resolves most issues without human intervention
```

**Scenario 3: Parallel Development**
```
# Problem: Work on multiple features simultaneously
# Without orchestrator: Manually manage branches, conflicts
# With orchestrator:
ao spawn frontend-agent    # Agent for frontend work
ao spawn backend-agent     # Agent for backend work
ao spawn database-agent    # Agent for database work

# Each agent:
# - Works in isolated worktree
# - Creates its own branch
# - Opens its own PRs
# - Handles its own CI/CD
# - You only review and merge
```

---

## Key Insights from X Posts

### AI Edge Computing Topics

From the provided X links, AI edge computing is a trending topic with discussions on:

1. **Agentic AI** - Autonomous agents that can think, plan, and execute
2. **Edge Computing** - Running AI models on edge devices (phones, IoT)
3. **Agent Orchestration** - Managing multiple agents working together
4. **Mini AI** - Running small AI models locally (on-device, no cloud)
5. **Memory Systems** - Persistent memory for AI across sessions
6. **Workflows** - Automated workflows combining multiple AI tools

### Best Practices for Solopreneurs

1. **Use Agent Orchestrators**
   - Don't manage agents manually
   - Use tools like ComposioHQ agent-orchestrator
   - Focus on strategy, not coordination

2. **Leverage Existing Tools**
   - GitHub Actions for CI/CD
   - Linear for issue tracking
   - Obsidian for knowledge management
   - tmux for terminal management

3. **Focus on Outcomes**
   - What problems do you solve?
   - What value do you create?
   - How much time/money do you save?

4. **Build for Scale**
   - Automate everything that can be automated
   - Use agents for repetitive tasks
   - Focus on strategic decisions

---

## Best Practices

### Agent Development

1. **Use Worktrees**
   - Isolated git trees prevent conflicts
   - Each agent has clean working environment

2. **Clear Boundaries**
   - Define what agents can and can't do
   - Set up escalation rules
   - Make approval workflows clear

3. **Monitoring is Essential**
   - Can't improve what you don't measure
   - Track agent performance, costs, outcomes
   - Set up alerts for failures and anomalies

### Architecture

1. **Separation of Concerns**
   - Business logic vs technical logic
   - Strategic vs operational decisions
   - Clear interfaces between layers

2. **Resilience**
   - Build for failures
   - Implement graceful degradation
   - Have rollback plans ready

3. **Observability**
   - Everything should be traceable
   - Logs, metrics, tracing
   - Dashboards for visibility

---

## Additional Resources

### GitHub Repositories

1. **ComposioHQ/agent-orchestrator**
   - https://github.com/ComposioHQ/agent-orchestrator
   - Agent orchestration for parallel coding
   - Best practices for multi-agent systems

2. **shanraisshan/claude-code-best-practice**
   - https://github.com/shanraisshan/claude-code-best-practice
   - Best practices for using Claude Code
   - Contribute to improve Claude Code usage

### Documentation

- **ComposioHQ Documentation:** Full setup guides and examples
- **CLAUDE.md:** Architecture and code conventions
- **SETUP.md:** Installation guide
- **TROUBLESHOOTING.md:** Common issues and fixes

---

## Integration with OpenClaw Daily Learning

This repository complements the OpenClaw Daily Learning knowledge base:

### Recommended Workflow

1. **Use Agent Orchestrator** for managing OpenClaw agents
2. **Reference Development Cycle** for building products
3. **Use Use Cases** for validated ideas
4. **Follow Architecture Patterns** for robust design

### Key Synergies

| Repository File | Agent Orchestrator Role |
|----------------|------------------------|
| Solopreneur_openclawArchitecture.md | Overall architecture strategy |
| Week3_4_Execution_Plan.md | OpenClaw CEO setup, foundation |
| Robust_Architecture_SDLC_Patterns.md | Design and SDLC patterns |
| Openclaw_Usecases.md | Agent use case catalog |

---

## Next Actions

### For This Week

1. [ ] Study ComposioHQ agent-orchestrator in depth
2. [ ] Clone and test agent-orchestrator setup
3. [ ] Integrate with OpenClaw development workflow
4. [ ] Set up monitoring and alerting
5. [ ] Document agent orchestration best practices

### For OpenClaw Setup

1. [ ] Create agent specifications for orchestrator
2. [ ] Set up worktree management
3. [ ] Configure CI/CD with GitHub Actions
4. [ ] Set up monitoring dashboard
5. [ ] Implement quality gates

---

## Success Criteria

- [ ] Agent orchestrator configured and tested
- [ ] Parallel agents working successfully
- [ ] CI/CD automation operational
- [ ] Monitoring and alerting active
- [ ] Best practices documented and followed
- [ ] Integration with OpenClaw workflow complete

---

**Last Updated:** 2026-02-24

**Repository:** https://github.com/itsaslamopenclawdata/OpenclawDailyLearning
