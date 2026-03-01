# 04 - Evidence Submission Strategies

## The Core Principle

**REAL WORK ONLY.**

The ODV crawls your URLs, reads your code, inspects your repositories, and evaluates your verification answers with an LLM. It cross-references your evidence against your context document and task history. Fake evidence is detected, results in immediate rejection, and damages your reputation score permanently.

This is not a system you can game with screenshots of someone else's terminal or placeholder repos. The ODV is specifically designed to detect this. One rejected task for fake evidence costs you far more than the PFT you would have earned -- it tanks your alignment score across all future evaluations.

---

## Evidence Types

### 1. URL Evidence (Best for Network Tasks)

URL evidence is the strongest form because the ODV can programmatically verify it.

**What to submit:**
- GitHub repository URL (public repo)
- Deployed application URL
- Published npm package URL
- Documentation site URL

**How to do it well:**

```bash
# Push your work to GitHub BEFORE submitting evidence
git add -A
git commit -m "feat: complete task implementation"
git push origin main

# Then submit the URL
npx pft-cli chat:send \
  --content "submit evidence for task abc123: https://github.com/yourname/project-name - [description of what you built]" \
  --wait
```

**What the ODV checks when crawling your URL:**
- Does the repo exist and is it public?
- Does the code match the task description?
- Is there a README with installation instructions?
- Does the code actually build/run?
- Is the commit history consistent (not a single dump of AI-generated code)?
- Does the author match your GitHub username from your context document?

**Pro tips for URL evidence:**
- Include a clean README with clear installation instructions
- Make sure `npm install && npm run build` (or equivalent) works without errors
- Add at least basic usage examples in the README
- If it's a CLI tool, include example commands and expected output
- If it's a deployed app, make sure it's actually accessible at the URL

### 2. Text Evidence (Best for Alpha Tasks)

Text evidence is a written description of your work, analysis, or findings. This is the primary format for alpha tasks where the deliverable is insight rather than code.

**What to submit:**
- 200-500 words of substantive content
- Specific details, numbers, and findings
- Clear attestation of what you did and what you found
- References to sources if applicable

**Example of strong text evidence:**

```
submit evidence for task def456:

Analysis of Post Fiat validator economics for the first 30 days:

I analyzed on-chain data from the PFTL testnet covering Feb 1-28, 2026.
Key findings:

1. Validator rewards follow a 55/45 split: 55% to validators over 6 years,
   45% to operators via task rewards. The 6-year vesting creates strong
   long-term alignment.

2. Current active validators: 12. Average uptime: 97.3%. The top 3
   validators by stake control 61% of network weight, indicating moderate
   centralization risk.

3. Task completion rates by type: network 73%, alpha 68%, personal 89%.
   Network tasks have the lowest completion rate but highest reward tier
   distribution (42% exceptional vs 28% for alpha).

4. Sybil detection appears to use submission pattern analysis. Operators
   with >5 tasks per day show 3.2x higher rejection rates than those
   with 1-3 tasks per day.

Methodology: Queried the XRPL testnet directly using xrpl.js, filtered
for PFT-tagged transactions, and aggregated by validator address and
task type. Raw data available in the accompanying spreadsheet.
```

**What makes this strong:**
- Specific numbers and percentages
- Clear methodology
- Actionable findings
- Not just description but analysis

### 3. Screenshot Evidence (Best for Personal Tasks)

Screenshot evidence is a visual proof of completion, typically for personal tasks where the deliverable is an activity rather than a digital artifact.

**What to submit:**
- Terminal output showing completed work
- App interface showing results
- Fitness tracker screenshot with date visible
- Reading app screenshot showing progress

**How to format:**

```
submit evidence for task ghi789: [screenshot description]

Terminal output from running the test suite:
- 47 tests passing, 0 failures
- Coverage: 92% statements, 88% branches
- Run date: 2026-03-01 14:30 PST
- Full output: [paste terminal output here]
```

**Tips for screenshot evidence:**
- Always include the date in the screenshot or description
- Show the full context, not just a cropped success message
- If it's a terminal screenshot, include the command that was run
- For personal tasks (gym, reading), include metrics (weight, reps, pages, time)

### 4. Code Evidence (Best for Small Deliverables)

For tasks where the deliverable is a small script, configuration, or code snippet, you can submit the code inline rather than pushing to a full repo.

**When to use:**
- Scripts under ~100 lines
- Configuration files
- Code patches or diffs
- One-off analysis scripts

**How to format:**

```
submit evidence for task jkl012:

Wrote a monitoring script that checks PFT validator uptime:

\`\`\`typescript
import { Client } from 'xrpl';

async function checkValidatorUptime(address: string): Promise<number> {
  const client = new Client('wss://pftl.postfiat.org');
  await client.connect();

  const response = await client.request({
    command: 'server_info',
  });

  const uptime = response.result.info.uptime;
  const uptimeHours = uptime / 3600;

  console.log(`Validator ${address}: ${uptimeHours.toFixed(1)}h uptime`);

  await client.disconnect();
  return uptimeHours;
}

// Run every 5 minutes
setInterval(() => checkValidatorUptime(process.env.VALIDATOR_ADDRESS!), 300000);
\`\`\`

The script connects to the PFTL testnet, queries server_info, and logs
uptime in hours. Designed to run as a background process with a 5-minute
polling interval.
```

---

## Evidence Quality Ladder

From weakest to strongest:

| Level | Description | Typical Rating |
|-------|-------------|----------------|
| 1 | "I did the thing" (no proof) | Rejected |
| 2 | Vague description with no specifics | Standard |
| 3 | Description with specific details | Good |
| 4 | Code/screenshot with description | Good-Exceptional |
| 5 | Public URL + README + description | Exceptional |

**Level 5 is the target.** For network tasks, always push to GitHub and include a README. The ODV can verify everything programmatically, which dramatically increases your chances of an exceptional rating.

---

## Evidence Submission Checklist

Before submitting evidence, verify:

- [ ] **For URL evidence:** Repo is public, README exists, code builds, URL is accessible
- [ ] **For text evidence:** 200+ words, specific numbers/findings, clear methodology
- [ ] **For screenshot evidence:** Date visible, full context shown, metrics included
- [ ] **For code evidence:** Code is complete, includes comments, includes usage instructions
- [ ] **For all types:** Description explains what was done, not just what was submitted
- [ ] **For all types:** Evidence directly addresses the task requirements (re-read the task description)
- [ ] **GitHub pushed:** If submitting a URL, make sure you `git push` before submitting

---

## Common Evidence Mistakes

| Mistake | Consequence | Fix |
|---------|-------------|-----|
| Submitting GitHub URL before pushing | ODV gets 404, task fails verification | Always `git push` first |
| Private repo | ODV can't access it | Make repo public |
| No README | ODV can't assess project quality | Add README with install/usage instructions |
| Vague description | ODV can't map evidence to task requirements | Be specific about what you built and how |
| Fake or fabricated evidence | Immediate rejection, permanent reputation damage | Do real work |
| Copy-pasting AI output without verification | ODV detects generic patterns | Customize, verify, and add your own analysis |

---

Previous: [Task Lifecycle](03-task-lifecycle.md) | Next: [Verification Mastery](05-verification-mastery.md)
