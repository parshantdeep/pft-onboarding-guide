# 06 - Alignment Score Mechanics

## What Is the Alignment Score?

Your alignment score is the single most important number on the Post Fiat network. It determines **86% of your leaderboard ranking**, which in turn determines your share of PFT rewards.

The alignment score measures how well your activity aligns with the network's goals: consistent, high-quality contributions across diverse task categories over sustained periods of time.

Think of it as a credit score for your operator reputation. It's easy to damage and slow to rebuild.

---

## The Formula: Two Components

The alignment score is a weighted combination of two evaluation methods:

```
Alignment Score = (0.65 * Deterministic Score) + (0.35 * LLM Evaluation Score)
```

### Deterministic Component (65%)

This is calculated algorithmically from on-chain data. No subjectivity, no LLM interpretation -- pure math.

**Factors:**

| Factor | What It Measures | Impact |
|--------|-----------------|--------|
| **Task Count** | Total completed tasks across all categories | More tasks = higher score (with diminishing returns) |
| **Category Diversity** | Distribution across network, alpha, and personal tasks | Operators active in all 3 categories score higher |
| **Recency** | When tasks were completed relative to scoring windows | Recent activity weighted heavily (see time windows below) |
| **Sybil Risk** | Similarity patterns suggesting automated/fake activity | High similarity between submissions = score penalty |

**Category Diversity Deep Dive:**

The system doesn't just count tasks -- it measures the Shannon entropy of your task type distribution. Maximum diversity (equal distribution across all 3 types) scores highest.

```
# Low diversity (bad): 10 network, 0 alpha, 0 personal
# Medium diversity: 7 network, 2 alpha, 1 personal
# High diversity (good): 5 network, 3 alpha, 2 personal
```

You don't need perfect balance, but doing ONLY one type of task penalizes your diversity signal significantly.

### LLM Evaluation Component (35%)

This is assessed by an LLM that reviews your profile, task history, and submission quality holistically.

**Factors:**

| Factor | What It Measures | Impact |
|--------|-----------------|--------|
| **Profile Quality** | How complete and specific your context document is | Strong profile = higher trust signal |
| **Sustained Engagement** | Consistency of activity over time (not burst farming) | Steady daily/weekly activity >> one-time bursts |
| **Contribution Quality** | Reward tier distribution (exceptional vs standard) | More exceptional ratings = higher quality signal |

The LLM evaluation is recalculated periodically and can shift based on your recent activity patterns. It's the "soft" component that rewards genuine, thoughtful engagement over mechanical task grinding.

---

## Score Time Windows

Your alignment score is recalculated using three overlapping time windows:

| Window | Weight | What It Captures |
|--------|--------|-----------------|
| **d7 (7-day)** | 55% | Your most recent week of activity |
| **d30 (30-day)** | 30% | Your activity over the past month |
| **d90 (90-day)** | 15% | Your long-term track record |

```
Final Score = (0.55 * d7_score) + (0.30 * d30_score) + (0.15 * d90_score)
```

**What this means in practice:**

- **Your last 7 days matter most.** A great week can significantly boost your score even if your 30-day and 90-day numbers are average.
- **Consistency is rewarded.** An operator who completes 2 tasks per week for 4 weeks scores higher than one who completes 8 tasks in one day and goes silent for 3 weeks -- even though the total task count is the same.
- **Recovery is possible.** If you have a bad week (low activity, rejected task), you can recover quickly because d7 is 55% of the score. But the d30 and d90 windows remember your history.

---

## Category Weights

Each task type contributes differently to your alignment score:

| Category | Weight | Contribution per Task |
|----------|--------|----------------------|
| **Network** | 0.5 | Highest impact per task |
| **Alpha** | 0.4 | Strong impact per task |
| **Personal** | 0.1 | Minimal impact per task |

**Practical math:**
- 1 network task contributes as much to your score as 5 personal tasks
- 1 alpha task contributes as much as 4 personal tasks
- A mix of 3 network + 2 alpha contributes as much as 23 personal tasks

This is why the guide emphasizes network tasks as the primary earning strategy.

---

## Optimization Strategies

### Strategy 1: Mix Task Types

Don't do only network tasks. The category diversity signal in the deterministic component rewards operators who demonstrate breadth.

**Recommended weekly mix:**
- 3-4 network tasks (primary earner, highest weight)
- 1-2 alpha tasks (diversity + good PFT return)
- 1 personal task (diversity signal, minimal time investment)

### Strategy 2: Spread Activity Across Days

The recency calculation and sybil detection both penalize burst farming. Completing 2 tasks per day for 5 days scores dramatically better than 10 tasks in one session.

**Anti-pattern to avoid:**
```
Mon: 0 tasks
Tue: 0 tasks
Wed: 0 tasks
Thu: 0 tasks
Fri: 10 tasks  <-- sybil flag, LLM downgrades sustained engagement
```

**Pattern to follow:**
```
Mon: 2 tasks
Tue: 1 task
Wed: 2 tasks
Thu: 1 task
Fri: 2 tasks  <-- consistent, no flags, high sustained engagement signal
```

### Strategy 3: Keep Context Document Updated

Your context document feeds the LLM evaluation component. An outdated or vague context document drags down your profile quality score.

After completing a significant task, update your context document to reflect the new capability:
- Built an MCP server? Add "MCP server development" to your skills
- Completed a validator analysis? Add "XRPL validator economics" to your expertise
- Published a tool? Add the URL to your verifiable work section

### Strategy 4: Earn Higher Reward Tiers

The LLM evaluation heavily weights your reward tier distribution. An operator with 5 exceptional ratings out of 8 tasks signals much higher quality than one with 8 standard ratings.

This is why the [Verification Mastery](05-verification-mastery.md) section exists. The difference between "good" and "exceptional" is often just the depth of your verification answer.

**Tier impact on alignment:**
| Distribution | Quality Signal |
|-------------|---------------|
| 7/8 exceptional | Very strong |
| 4/8 exceptional, 4/8 good | Strong |
| 8/8 standard | Moderate |
| 6/8 standard, 2/8 rejected | Weak |

### Strategy 5: Avoid Sybil Similarity

The deterministic component includes a sybil risk factor that detects submission pattern similarity. If your tasks look like they were generated by an automated script (same structure, same timing, same evidence format), the system flags them.

**How to avoid sybil flags:**
- Vary your task request descriptions (don't use the same template)
- Vary your evidence format (URLs for some, text for others)
- Vary your submission timing (not exactly every 6 minutes)
- Vary your verification answer style and depth
- Do genuinely different work (the best anti-sybil measure)

### Strategy 6: Maintain a Positive Trajectory

The d7 window is 55% of your score. This means your score can move significantly week over week. Use this to your advantage:

- After a slow week, ramp up activity the following week
- After a rejected task, immediately submit a high-quality task to dilute the negative signal
- Track your score weekly and correlate changes with your activity patterns

---

## Score Monitoring

Check your current standing on the leaderboard:
- [https://tasknode.postfiat.org](https://tasknode.postfiat.org) -- leaderboard page
- Your position reflects your alignment score relative to other operators
- Score updates are not instant -- they recalculate periodically

---

## Summary Table

| Component | Weight | Key Factors |
|-----------|--------|-------------|
| Deterministic | 65% | Task count, category diversity, recency, sybil risk |
| LLM Evaluation | 35% | Profile quality, sustained engagement, contribution quality |
| d7 window | 55% | Last 7 days of activity |
| d30 window | 30% | Last 30 days of activity |
| d90 window | 15% | Last 90 days of activity |
| Network tasks | 0.5 weight | Infrastructure, tools, ecosystem building |
| Alpha tasks | 0.4 weight | Research, analysis, intelligence |
| Personal tasks | 0.1 weight | Self-development, skills, health |
| Alignment in leaderboard | 86% | Alignment score as fraction of total leaderboard ranking |

---

## Checklist

- [ ] Understand the two-component formula (65% deterministic + 35% LLM)
- [ ] Know the three time windows and their weights (d7: 55%, d30: 30%, d90: 15%)
- [ ] Know the category weights (network 0.5, alpha 0.4, personal 0.1)
- [ ] Planning a mixed weekly task schedule (not all one type)
- [ ] Spreading tasks across days (not burst farming)
- [ ] Keeping context document updated after major tasks
- [ ] Aiming for exceptional tier on every task

---

Previous: [Verification Mastery](05-verification-mastery.md) | Next: [Common Pitfalls](07-common-pitfalls.md)
