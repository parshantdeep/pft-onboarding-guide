# 02 - Context Document Best Practices

## What Is the Context Document?

Your context document is a structured profile that the ODV (On-chain Decentralized Verifier) reads every time it generates tasks for you. Think of it as your resume for the task generation system. The better your context document, the more relevant and higher-quality tasks you receive.

The ODV uses your context document to:
- Generate tasks matched to your actual skills
- Assess whether your submitted evidence aligns with your stated capabilities
- Calibrate verification questions to your domain expertise
- Evaluate alignment between your goals and your task activity

**Bottom line:** A vague context document produces vague, low-value tasks. A specific context document produces tasks you can crush for exceptional ratings.

---

## Scoring Rubric

The ODV evaluates context documents on four dimensions:

| Dimension | Weight | What It Measures |
|-----------|--------|-----------------|
| **Completeness** | 25% | Does it cover skills, experience, goals, and preferences? |
| **Specificity** | 30% | Are claims concrete (languages, frameworks, repos) or vague ("I know coding")? |
| **Task Alignment** | 25% | Can the system generate actionable tasks from this? |
| **Actionability** | 20% | Does it enable the ODV to verify your work against your stated capabilities? |

---

## Example: WEAK Context Document (~20/100)

```
I'm a developer interested in crypto. I can code in various languages
and have experience with blockchain projects. I want to contribute to
the Post Fiat ecosystem and earn PFT. I'm good at building things and
learning new technologies.
```

**Why this scores poorly:**

- **Completeness (5/25):** No specific skills, no platforms, no prior work
- **Specificity (3/30):** "Various languages" and "blockchain projects" tell the ODV nothing
- **Task Alignment (7/25):** The ODV can't generate targeted tasks from "I can code"
- **Actionability (5/20):** No way to verify claims -- no repos, no usernames, no concrete work

**Result:** The ODV generates generic tasks like "Write a blog post about blockchain" or "Create a simple script." These are low-weight, hard to score exceptional on, and waste your time.

---

## Example: STRONG Context Document (~90/100)

```
## Identity
- GitHub: your-github-username
- X/Twitter: your-x-handle
- XRPL Wallet: rYourWalletAddressHere

## Technical Skills
- Languages: TypeScript (primary), Python, Rust (learning)
- Frameworks: Node.js, React, Express, Oclif (CLI framework)
- Infrastructure: Docker, GitHub Actions CI/CD, Vercel, Railway
- Blockchain: XRPL (transactions, hooks), Solidity (basic)
- AI/ML: Claude API, OpenAI API, MCP (Model Context Protocol) server development

## What I've Built (Verifiable)
- my-mcp-server: MCP server wrapping a REST API
  (https://github.com/your-username/my-mcp-server)
- my-dashboard.example.com: Real-time portfolio dashboard
- trading-bot: Automated trading bot for DeFi protocols
- discord-bot: Discord bot with multi-channel integration and CLI interface

## Current Goals
- Build tools that make the Post Fiat ecosystem more accessible
- Create developer documentation and onboarding resources
- Explore validator operations on the PFT network
- Ship 2-3 open-source projects per month

## Work Preferences
- I prefer network tasks (building tools, infrastructure, documentation)
- I ship fast: typical project from idea to deployed in 4-12 hours
- I use AI coding assistants as my primary development environment
- All my work is open-source and pushed to GitHub immediately

## What I'm NOT Interested In
- Pure research tasks without a concrete deliverable
- Social media engagement tasks
- Tasks requiring platforms I don't use
```

**Why this scores well:**

- **Completeness (23/25):** Covers identity, skills, prior work, goals, preferences, and anti-preferences
- **Specificity (27/30):** Named languages, frameworks, actual repos with URLs, concrete projects
- **Task Alignment (23/25):** The ODV can generate tasks like "Build an XRPL transaction explorer" or "Create CI/CD pipeline for PFT validator" -- tasks this person can actually do well
- **Actionability (18/20):** Every claim is verifiable via GitHub repos, live URLs, or on-chain history

**Result:** The ODV generates targeted tasks matched to actual capabilities. Higher relevance means faster completion, better evidence, and more exceptional ratings.

---

## Tips for a High-Scoring Context Document

### 1. Include Your GitHub Username
The ODV crawls GitHub when verifying URL evidence. If your GitHub username isn't in your context document, the system can't connect your submissions to your identity.

```
GitHub: your-github-username
```

### 2. List Your Specific Tech Stack
Don't say "I know web development." Say exactly what you use:

```
Frontend: React, Next.js, Tailwind CSS
Backend: Node.js, Express, PostgreSQL, Redis
DevOps: Docker, GitHub Actions, Vercel, Railway
```

### 3. Reference What You've Already Built
The ODV trusts operators who have verifiable prior work. Link to repos, deployed apps, or published packages:

```
- my-mcp-server (https://github.com/your-username/my-mcp-server) - MCP server for Task Node
- my-dashboard.example.com - Real-time portfolio dashboard
```

### 4. State Your Goals Clearly
The ODV generates tasks aligned with your stated goals. If your goal is "build PFT ecosystem tools," you'll get tasks like "Create a PFT wallet CLI." If your goal is vague, you'll get vague tasks.

### 5. Include Anti-Preferences
Telling the system what you don't want is almost as valuable as telling it what you do want. This prevents low-relevance tasks from clogging your queue.

### 6. Update Regularly
Your context document should evolve as you complete tasks and build new skills. After completing a major task, update your context document to reflect the new capability. This creates a positive feedback loop: better context -> better tasks -> better results -> better context.

---

## How to Update Your Context Document

1. Go to [https://tasknode.postfiat.org/context](https://tasknode.postfiat.org/context) (or navigate to the Context page from your dashboard)
2. Edit the text field with your updated context
3. Save

Changes take effect on the next task generation cycle. There's no need to wait -- the ODV reads your latest context document every time it generates a new task for you.

---

## Checklist

- [ ] Context document includes your GitHub username
- [ ] Specific languages, frameworks, and tools listed (not "various" or "multiple")
- [ ] At least 2-3 verifiable prior projects with links
- [ ] Clear goals that map to task types (network, alpha, personal)
- [ ] Anti-preferences stated to filter out irrelevant tasks
- [ ] Document reviewed and updated after each major task completion

---

Previous: [Getting Started](01-getting-started.md) | Next: [Task Lifecycle](03-task-lifecycle.md)
