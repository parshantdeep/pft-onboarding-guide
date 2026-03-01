# 03 - Task Lifecycle Deep Dive

## Overview

Every PFT-earning task follows a predictable lifecycle with 6 phases. Understanding each phase -- and what the ODV expects at each stage -- is the difference between standard and exceptional ratings.

A typical full cycle takes **5-6 minutes** if you have the work already done, or **hours to days** if the task requires building something substantial. The phases themselves are fast; the actual work is where your time goes.

---

## The 6 Phases

```
Request --> Propose --> Accept --> Work --> Evidence + Verify --> Reward
   |           |          |         |            |                  |
   v           v          v         v            v                  v
 You ask    ODV sends   You say   You do     You submit         PFT lands
 for work   a proposal  "yes"     the work   proof + answer     in wallet
                                              verification Q
```

### Phase 1: Request

You send a message to the ODV requesting a specific type of task. The more specific your request, the better the task you'll receive.

**Magic phrases** the ODV recognizes:
- `"request a network task: [description]"` -- tasks that build Post Fiat infrastructure (weight: 0.5)
- `"request an alpha task: [description]"` -- tasks that generate alpha/insights (weight: 0.4)
- `"request a personal task: [description]"` -- personal development tasks (weight: 0.1)

The description after the colon shapes what the ODV proposes. Compare:
- Weak: `"request a network task: do something useful"`
- Strong: `"request a network task: build an MCP server that wraps the Task Node REST API for use in Claude Code and other LLM-powered IDEs"`

### Phase 2: Propose

The ODV reads your context document, your request, and your task history, then generates a specific task proposal. The proposal includes:
- Task ID (used for all subsequent commands)
- Task description (what to build/do)
- Task type (network, alpha, or personal)
- Expected deliverables
- Evaluation criteria

You'll receive the proposal as a response to your request message.

### Phase 3: Accept

Review the proposal. If it matches your capabilities and you want to do it, accept it:

```bash
npx pft-cli tasks:accept <task-id>
```

If the proposal doesn't match what you wanted, you can request a different task instead. There's no penalty for not accepting a proposal.

### Phase 4: Work

This is where you actually do the thing. Build the tool, write the analysis, complete the exercise -- whatever the task requires.

**Key insight:** The work phase has no time limit, but the scoring windows favor recency. A task completed within hours of acceptance signals higher engagement than one that sits for days.

### Phase 5: Evidence + Verification

This is a two-part phase:

**Part A -- Submit Evidence:**
You submit proof that you completed the task. Evidence types include URLs (GitHub repos, deployed apps), text descriptions, code snippets, or screenshots. See [Evidence Strategies](04-evidence-strategies.md) for details.

**Part B -- Answer Verification Question:**
After you submit evidence, the ODV generates a targeted verification question based on your evidence. You must answer it. The quality of your answer is a major factor in your reward tier. See [Verification Mastery](05-verification-mastery.md) for details.

### Phase 6: Reward

The ODV evaluates your evidence and verification answer, assigns a rating tier, and distributes PFT to your wallet.

**Reward tiers** (from highest to lowest):
| Tier | Description | PFT Range |
|------|-------------|-----------|
| Exceptional | Outstanding quality, exceeds expectations | Highest |
| Good | Solid work, meets all requirements | Medium |
| Standard | Acceptable, meets minimum bar | Lower |
| Rejected | Fake evidence, incomplete, or off-topic | 0 PFT |

---

## Task Types and Weights

| Type | Weight | What It Means | Examples |
|------|--------|---------------|----------|
| **Network** | 0.5 | Builds Post Fiat infrastructure, tools, or ecosystem | MCP servers, CLI tools, documentation, integrations, validator setup |
| **Alpha** | 0.4 | Generates insights, analysis, or market intelligence | Research reports, data analysis, competitive intelligence, strategy memos |
| **Personal** | 0.1 | Personal development, skills, health | Reading summaries, exercise logs, meditation reports |

**Weight matters for alignment scoring.** A network task contributes 5x more to your alignment score's category diversity signal than a personal task. See [Alignment Scoring](06-alignment-scoring.md) for the full formula.

> **Strategy:** Prioritize network tasks for maximum PFT earnings and alignment score impact. Mix in alpha tasks for diversity. Use personal tasks sparingly -- they help with category diversity but contribute minimally to score.

---

## Full CLI Example Flow

Here's a complete task cycle from request to reward, with actual commands:

### 1. Request a Task

```bash
npx pft-cli chat:send \
  --content "request a network task: build a comprehensive onboarding guide for new Post Fiat operators, covering CLI setup, context documents, task lifecycle, evidence strategies, and alignment scoring" \
  --context "I am a TypeScript developer who has completed 8 tasks with 7 exceptional ratings. I have deep experience with the pft-test-client CLI and Task Node API." \
  --wait
```

The `--wait` flag blocks until the ODV responds with a proposal. Without it, you'd need to poll for the response.

The `--context` flag provides additional context for this specific request, supplementing your context document.

### 2. Review the Proposal

The ODV responds with a task proposal. Note the task ID from the response.

### 3. Accept the Task

```bash
npx pft-cli tasks:accept abc123def456
```

### 4. Get Task Details

```bash
npx pft-cli tasks:get abc123def456
```

This shows the full task description, deliverables, and evaluation criteria. Read it carefully before starting work.

### 5. Do the Work

Build the thing. Write the analysis. Complete the exercise. Push to GitHub. Deploy if applicable.

### 6. Submit Evidence

```bash
npx pft-cli chat:send \
  --content "submit evidence for task abc123def456: https://github.com/your-username/pft-onboarding-guide - Complete onboarding guide with 7 sections covering setup, context documents, task lifecycle, evidence strategies, verification, alignment scoring, and common pitfalls." \
  --wait
```

### 7. Wait for Verification Question

The ODV processes your evidence and generates a verification question. This typically takes 30-120 seconds.

```bash
npx pft-cli verify:wait abc123def456
```

### 8. Answer the Verification Question

```bash
npx pft-cli verify:respond \
  --task-id abc123def456 \
  --type text \
  --response "The guide is structured into 7 markdown files under the guide/ directory. Section 06 on alignment scoring covers the dual-component formula: 65% deterministic (task count, category diversity, recency, sybil risk) and 35% LLM evaluation (profile quality, sustained engagement, contribution quality). The score windows are d7 at 55%, d30 at 30%, and d90 at 15%. I chose this structure because new operators consistently ask about scoring mechanics in Discord but there's no single reference document. The specific numbers come from analyzing the leaderboard API responses and correlating with my own score changes over 8 tasks."
```

### 9. Receive Reward

The ODV evaluates everything and distributes PFT. Check your task history:

```bash
npx pft-cli tasks:history
```

---

## CLI Command Reference

| Command | Purpose |
|---------|---------|
| `chat:send --content "..." --wait` | Send a message to the ODV and wait for response |
| `chat:send --content "..." --context "..." --wait` | Send with additional context |
| `tasks:list` | List available/pending tasks |
| `tasks:accept <id>` | Accept a task proposal |
| `tasks:get <id>` | Get full task details |
| `tasks:history` | View completed tasks and rewards |
| `verify:wait <id>` | Wait for a verification question |
| `verify:respond --task-id <id> --type text --response "..."` | Answer a verification question |
| `auth:set-token` | Set/refresh JWT token |
| `auth:status` | Check credential status |

---

## Timing Tips

- **Request to proposal:** Usually < 30 seconds
- **Evidence to verification question:** 30-120 seconds
- **Verification answer to reward:** 1-5 minutes
- **Full cycle (with prepared work):** 5-6 minutes
- **Full cycle (with real building):** Hours to days depending on scope

The phases are fast. Don't try to rush the work itself -- the ODV rewards quality, not speed.

---

## Checklist

- [ ] Understand all 6 phases and what each requires
- [ ] Know the three task types and their weights
- [ ] Can construct a specific task request with magic phrase
- [ ] Know how to use `--wait` and `--context` flags
- [ ] Can navigate the full CLI flow from request to reward

---

Previous: [Context Document](02-context-document.md) | Next: [Evidence Strategies](04-evidence-strategies.md)
