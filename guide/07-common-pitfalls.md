# 07 - Common Pitfalls

## Overview

These are the mistakes that consistently cost operators PFT, tank alignment scores, or waste time. Every pitfall listed here is based on real observed patterns -- either firsthand experience or behavior documented in the Post Fiat Discord.

Read this section once before your first task. Re-read it after your fifth task. Most operators make at least one of these mistakes in their first week.

---

## Pitfall 1: Submitting Fake Evidence

**What happens:** You submit evidence for work you didn't actually do -- a placeholder repo, fabricated terminal output, or someone else's code.

**Consequence:** Immediate task rejection (0 PFT). More importantly, the rejection enters your permanent task history, which the LLM evaluation component reviews when calculating your alignment score. One fake submission can downgrade your "contribution quality" signal for weeks.

**Why it doesn't work:** The ODV is not a rubber stamp. For URL evidence, it:
- Crawls the actual URL and reads the code
- Checks commit history and authorship
- Verifies that the code matches the task description
- Cross-references your GitHub username against your context document
- Generates a verification question specifically designed to test whether you built it

For text evidence, the LLM evaluator checks for:
- Generic/templated language patterns
- Claims that contradict your context document
- Specificity level (fake evidence is almost always vague)

**Fix:** Do real work. If a task is beyond your capabilities, don't accept it. Request a different task that matches your skills.

---

## Pitfall 2: All Tasks Same Type

**What happens:** You only do network tasks because they have the highest weight (0.5), ignoring alpha and personal tasks entirely.

**Consequence:** Your category diversity score in the deterministic component drops. The system specifically measures Shannon entropy across task types. Zero-entropy (single category) is the worst possible diversity score.

**The math:**
- 10 network tasks, 0 alpha, 0 personal: diversity score ~0.0
- 7 network, 2 alpha, 1 personal: diversity score ~0.8
- 5 network, 3 alpha, 2 personal: diversity score ~0.95

Even though network tasks have 5x the weight of personal tasks, the diversity penalty from doing ONLY network tasks outweighs the per-task weight advantage.

**Fix:** Maintain a weekly mix. Target 60-70% network, 20-30% alpha, 10% personal. One personal task per week is enough to maintain a healthy diversity signal.

---

## Pitfall 3: Burst Farming

**What happens:** You complete 10+ tasks in a single session, trying to maximize output in minimal time.

**Consequence:** Two penalties hit simultaneously:
1. **Sybil similarity flag:** Rapid-fire submissions with similar patterns trigger the sybil detection in the deterministic score
2. **LLM sustained engagement downgrade:** The LLM evaluator sees your activity as a one-time burst rather than consistent engagement, which lowers your "sustained engagement" factor

**The data:** Operators with >5 tasks per day show approximately 3x higher rejection rates than those with 1-3 tasks per day. The effect is even stronger for tasks completed within minutes of each other.

**Fix:** Spread your activity across the week. 2 tasks per day, 5 days per week (10 total) scores dramatically higher than 10 tasks in one Friday afternoon session -- even though the task count is identical.

---

## Pitfall 4: Vague Context Document

**What happens:** Your context document says things like "I'm a developer interested in crypto" without specific skills, repos, or goals.

**Consequence:** The ODV generates generic, low-value tasks that are:
- Hard to score exceptional on (because the task itself is vague)
- Poorly matched to your actual abilities
- More likely to result in mediocre evidence and standard/good ratings

Additionally, the LLM evaluation component scores your "profile quality" as low, which directly reduces the 35% LLM portion of your alignment score.

**Fix:** Follow the context document guide in [Section 02](02-context-document.md). Minimum requirements:
- GitHub username
- Specific tech stack (languages, frameworks, tools)
- 2-3 verifiable prior projects with URLs
- Clear goals mapped to task types
- Anti-preferences to filter irrelevant tasks

---

## Pitfall 5: Generic Verification Answers

**What happens:** The ODV asks "Walk through your authentication implementation" and you respond with "I implemented JWT authentication using standard patterns."

**Consequence:** You get rated "standard" or "good" instead of "exceptional." Over 8 tasks, the difference between consistent "good" and consistent "exceptional" can be thousands of PFT.

**Why it matters beyond individual tasks:** Your reward tier distribution is a factor in the LLM evaluation. An operator with 7/8 exceptional ratings gets a much higher "contribution quality" signal than one with 8/8 standard ratings, even though both completed the same number of tasks.

**Fix:** Follow the verification answer rules in [Section 05](05-verification-mastery.md):
- Reference specific file names and line numbers
- Include actual terminal output or code snippets
- Explain WHY you made architectural decisions
- Be honest about limitations
- Write 150-400 words of dense, specific content

---

## Pitfall 6: JWT Expiring Mid-Session

**What happens:** You're in the middle of a task cycle and suddenly get authentication errors. Your JWT token has expired.

**Consequence:** You can't submit evidence or answer verification questions until you refresh the token. If you have a pending verification question, the delay can impact your response timing (though this is a minor factor compared to answer quality).

**How JWTs expire:** The token issued when you log in to Task Node has a limited lifetime (typically 24 hours). The CLI reads this token from `~/.pft-tasknode/config.json`. When it expires, all API calls fail.

**Fix:**
1. Log in to [https://tasknode.postfiat.org](https://tasknode.postfiat.org) in your browser
2. Open DevTools (F12 or Cmd+Shift+I)
3. Go to Application > Local Storage > `https://tasknode.postfiat.org`
4. Copy the fresh JWT token
5. Run `npx pft-cli auth:set-token` and paste the new token

**Prevention:** At the start of each session, run `npx pft-cli auth:status` to check token expiry. If it's expiring within the next hour, refresh proactively.

---

## Pitfall 7: Not Pushing to GitHub Before Submitting URL Evidence

**What happens:** You submit a GitHub URL as evidence, but you forgot to `git push`. The ODV crawls the URL and gets a 404 or sees an outdated version of the code.

**Consequence:** The ODV can't verify your evidence. At best, you get a lower rating because the evidence is incomplete. At worst, the task is rejected.

**This is embarrassingly common.** The flow is:
1. Build the thing locally
2. Commit locally
3. Submit evidence with GitHub URL
4. ... forgot to push

**Fix:** Make this your evidence submission ritual:

```bash
# Before EVERY evidence submission
git add -A
git commit -m "feat: complete task implementation"
git push origin main

# Verify it's live
# Open the URL in your browser. Can you see the code? Good.

# NOW submit evidence
npx pft-cli chat:send --content "submit evidence for task abc123: https://github.com/..." --wait
```

---

## Pitfall Summary

| # | Pitfall | Impact | Severity |
|---|---------|--------|----------|
| 1 | Fake evidence | Rejection + permanent reputation damage | Critical |
| 2 | Single task type | Category diversity penalty | High |
| 3 | Burst farming | Sybil flag + engagement downgrade | High |
| 4 | Vague context document | Bad tasks + low profile score | High |
| 5 | Generic verification answers | Standard/good instead of exceptional | Medium |
| 6 | JWT expiration | Blocked until token refresh | Medium |
| 7 | Forgot to git push | ODV can't crawl evidence URL | Medium |

---

## The Meta-Pitfall

The biggest pitfall isn't any individual mistake. It's **optimizing for PFT instead of optimizing for real contribution.**

The alignment scoring system is specifically designed to reward operators who build real things that benefit the network. Every optimization in this guide -- task mixing, spread activity, strong verification answers -- is really just a description of what genuine, consistent contribution looks like.

The operators at the top of the leaderboard aren't there because they gamed the system. They're there because they built useful tools, wrote valuable analysis, and showed up consistently. The scoring system just measures that.

**Do real work. The score follows.**

---

Previous: [Alignment Scoring](06-alignment-scoring.md) | Back to [README](../README.md)
