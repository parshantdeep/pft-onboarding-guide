# Post Fiat Operator Onboarding Guide

A comprehensive, practical guide to earning PFT on the [Post Fiat Task Node](https://tasknode.postfiat.org). Written from real experience: **61,437 PFT earned across 8 tasks with 7 exceptional ratings**.

This is not theory. Every strategy, example, and recommendation in this guide comes from actual task submissions, verification responses, and alignment score optimization on the live network.

---

## Who This Is For

- Developers, researchers, and builders who want to earn PFT by completing tasks on the Post Fiat network
- Anyone who has signed up for Task Node and wants to maximize their output and alignment score
- People evaluating Post Fiat and want to understand how the operator incentive system works

## Guide Sections

| # | Section | What You'll Learn |
|---|---------|-------------------|
| 1 | [Getting Started](guide/01-getting-started.md) | Account setup, wallet activation, CLI installation, credential configuration |
| 2 | [Context Document](guide/02-context-document.md) | How to write a context document that generates high-quality tasks (weak vs strong examples) |
| 3 | [Task Lifecycle](guide/03-task-lifecycle.md) | The 6 phases from request to reward, task types, weights, and full CLI workflow |
| 4 | [Evidence Strategies](guide/04-evidence-strategies.md) | How to submit evidence that passes ODV verification (URLs, text, code, screenshots) |
| 5 | [Verification Mastery](guide/05-verification-mastery.md) | How to answer verification questions for exceptional ratings |
| 6 | [Alignment Scoring](guide/06-alignment-scoring.md) | The scoring formula, category weights, time windows, and optimization strategies |
| 7 | [Common Pitfalls](guide/07-common-pitfalls.md) | Mistakes that kill your score and how to avoid them |

## Quick Start (5 Minutes)

```bash
# 1. Sign up at https://tasknode.postfiat.org (X or GitHub auth)
# 2. Clone and build the CLI
git clone https://github.com/postfiatorg/pft-test-client.git
cd pft-test-client/ts
npm install
npm run build

# 3. Set your credentials
npx pft-cli auth:set-token    # paste JWT from browser DevTools
export PFT_WALLET_MNEMONIC="your twelve word mnemonic phrase here"

# 4. Verify setup
npx pft-cli auth:status

# 5. Request your first task
npx pft-cli chat:send --content "request a network task: build an MCP server for Task Node API" --context "I am a TypeScript developer with experience building MCP servers and CLI tools"

# 6. Read the full guide for everything else
```

## Key Numbers

| Metric | Value |
|--------|-------|
| Time per task cycle | 5-6 minutes (request through verification) |
| Alignment score weight in leaderboard | 86% |
| Network task weight | 0.5 |
| Alpha task weight | 0.4 |
| Personal task weight | 0.1 |
| Exceptional reward tier | Top tier (highest PFT per task) |
| Score window: 7-day | 55% weight |
| Score window: 30-day | 30% weight |
| Score window: 90-day | 15% weight |

## The Single Most Important Thing

**Do real work. Submit real evidence. The ODV (On-chain Decentralized Verifier) crawls your URLs, reads your code, and evaluates your verification answers with an LLM. There are no shortcuts. The operators who earn the most are the ones who build things that actually work.**

---

## License

[MIT](LICENSE)
