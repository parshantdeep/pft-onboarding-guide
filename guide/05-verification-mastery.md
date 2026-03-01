# 05 - Verification Mastery: Answering for Exceptional Ratings

## How Verification Works

After you submit evidence, the ODV does two things:

1. **Processes your evidence** -- crawls URLs, reads code, analyzes text
2. **Generates a targeted verification question** -- designed to test whether you actually did the work

The verification question is not random. It's specifically crafted based on your evidence to probe the areas where fake submissions would break down. Someone who actually built the thing can answer effortlessly. Someone who faked it cannot.

Your answer to the verification question is a major factor in your reward tier. The difference between a "good" and "exceptional" rating often comes down to the depth and specificity of this single answer.

---

## The Verification Flow

```bash
# 1. Submit evidence (see previous section)
npx pft-cli chat:send --content "submit evidence for task abc123: ..." --wait

# 2. Wait for the verification question
npx pft-cli verify:wait abc123

# 3. Read the question carefully

# 4. Respond with depth and specificity
npx pft-cli verify:respond \
  --task-id abc123 \
  --type text \
  --response "Your detailed answer here..."
```

The `verify:wait` command blocks until the ODV generates the question (typically 30-120 seconds). You can also poll manually if you prefer:

```bash
npx pft-cli tasks:get abc123  # check if verification question is present
```

---

## Common Question Patterns

Based on 8 completed tasks, these are the verification question patterns the ODV uses:

### Pattern 1: "Show Terminal Output of X"

> "Can you show the terminal output of running your MCP server and demonstrate a successful tool call?"

The ODV wants to see that your code actually runs. This is where fake evidence collapses -- if you didn't build it, you can't show real terminal output.

### Pattern 2: "Walk Through How Y Works"

> "Walk through how your authentication flow handles JWT expiration and token refresh."

The ODV wants architectural understanding, not just "it works." This tests whether you made design decisions or just copied boilerplate.

### Pattern 3: "Provide a Specific Example of Z"

> "Provide a specific example of how your alignment scoring documentation helped a new operator improve their score."

The ODV wants concrete evidence of impact, not theoretical claims.

### Pattern 4: "What Was the Most Challenging Part?"

> "What was the most challenging technical problem you encountered and how did you solve it?"

This tests depth of engagement. If you actually built something, you hit real problems. If you faked it, your answer will be generic.

### Pattern 5: "How Does This Compare to X?"

> "How does your implementation differ from the existing Python SDK approach?"

The ODV tests your awareness of the broader ecosystem and your ability to articulate design trade-offs.

---

## Exceptional vs Mediocre Answers: Side by Side

### Question: "Walk through the architecture of your MCP server and explain how it handles task lifecycle operations."

### Mediocre Answer (rates "Standard" or "Good"):

```
The MCP server connects to the Task Node API and provides tools for
managing tasks. It has tools for sending messages, listing tasks,
accepting tasks, and checking verification status. The server uses
the MCP protocol to communicate with Claude Code. Authentication
is handled via JWT tokens.
```

**Why this is mediocre:**
- Describes WHAT it does but not HOW
- No specific file names, function names, or architectural decisions
- Could have been written by reading the README without building anything
- No mention of challenges, trade-offs, or design decisions

### Exceptional Answer (rates "Exceptional"):

```
The MCP server is structured around 15 tools defined in src/tools/,
each mapping to a specific Task Node API endpoint. The core architecture
follows the MCP SDK pattern: a Server instance registers tool handlers
via server.setRequestHandler(ListToolsRequestSchema, ...) and
server.setRequestHandler(CallToolRequestSchema, ...).

The task lifecycle is handled by 5 tools:
- task_send_message: wraps POST /api/chat with --content and --context params
- task_list: wraps GET /api/tasks with filtering by status
- task_accept: wraps POST /api/tasks/{id}/accept
- task_get: wraps GET /api/tasks/{id} for full detail including verification Q
- task_verify_respond: wraps POST /api/tasks/{id}/verify

Authentication uses a two-layer approach: JWT for API calls (stored in
~/.pft-tasknode/config.json, loaded at server startup) and wallet mnemonic
for on-chain signing (read from PFT_WALLET_MNEMONIC env var, never persisted
to disk).

The hardest design decision was handling the --wait flag for chat:send.
The Task Node API is asynchronous -- you send a message and poll for the
response. I implemented a polling loop in src/utils/poller.ts with
exponential backoff (initial 2s, max 30s, timeout 120s) that checks
GET /api/chat/{conversationId}/latest until the ODV response appears.
This was necessary because MCP tool calls are synchronous from the
client's perspective -- Claude Code expects a response, not a callback.

The server is published to npm as @postfiatorg/pft-tasknode-mcp and
can be installed via npx. Configuration is loaded from ~/.pft-tasknode/
on first run.
```

**Why this rates exceptional:**
- Specific file paths (`src/tools/`, `src/utils/poller.ts`)
- Actual API endpoints and HTTP methods
- Architectural decisions explained with reasoning ("MCP tool calls are synchronous")
- Honest about the hardest problem and how it was solved
- Implementation details that only someone who built it would know
- Package name and installation method

---

## The 5 Rules for Exceptional Verification Answers

### Rule 1: Reference Specific File Names and Line Numbers

Don't say "the authentication module." Say "src/auth/jwt-handler.ts, specifically the refreshToken() function on line 47." The ODV can cross-reference this against your code.

### Rule 2: Include Actual Terminal Output or Code Snippets

Copy-paste real terminal output. Include actual code snippets from your implementation. This is trivial if you built it and impossible if you didn't.

```
$ npx pft-cli auth:status
Auth Status:
  JWT Token: Set (expires in 23h 45m)
  Wallet: rYourWalletAddressHere
  Network: PFTL Testnet
  Status: Ready
```

### Rule 3: Explain Architectural Decisions, Not Just What You Built

The ODV has already seen WHAT you built (from your evidence). The verification question tests WHETHER YOU understand WHY you built it that way.

- Weak: "I used Express for the backend."
- Strong: "I chose Express over Fastify because the MCP SDK examples use Express and I wanted to minimize integration risk for a first version. If performance becomes an issue, the route handlers are isolated in src/routes/ and can be migrated without changing the tool logic."

### Rule 4: Be Honest If Something Isn't Implemented

If the ODV asks about a feature you didn't build, say so. Honest answers about limitations score higher than fabricated claims about features that don't exist.

- Weak: "Yes, the server handles rate limiting and retry logic." (when it doesn't)
- Strong: "Rate limiting isn't implemented yet. Currently, if the Task Node API returns 429, the tool call fails with an error message. I'd add retry logic with exponential backoff as a next step -- the polling infrastructure in src/utils/poller.ts already has the backoff logic that could be extracted."

The ODV respects intellectual honesty. It destroys trust when it detects fabrication.

### Rule 5: Write 150-400 Words

Short answers (under 50 words) signal low effort. Excessively long answers (over 500 words) dilute signal. The sweet spot is 150-400 words of dense, specific content.

---

## Verification Response Template

Use this structure when answering verification questions:

```
1. Direct answer (1-2 sentences addressing the question directly)

2. Technical details (specific files, functions, APIs, or methods)

3. Design reasoning (why you chose this approach over alternatives)

4. Honest limitations (what's not implemented, what you'd improve)

5. Evidence of real engagement (terminal output, specific numbers, things
   only someone who built it would know)
```

---

## Timing

- Verification questions appear 30-120 seconds after evidence submission
- You should answer within minutes, not hours (signals engagement)
- There's no strict time limit, but faster responses correlate with higher ratings
- Use `verify:wait` to block until the question appears, then respond immediately

---

## Checklist

- [ ] Used `verify:wait` to catch the question immediately
- [ ] Answer references specific file names and/or line numbers
- [ ] Answer includes actual terminal output or code snippets
- [ ] Answer explains WHY, not just WHAT
- [ ] Answer is honest about limitations
- [ ] Answer is 150-400 words of dense, specific content
- [ ] Answer directly addresses the specific question asked (re-read it before responding)

---

Previous: [Evidence Strategies](04-evidence-strategies.md) | Next: [Alignment Scoring](06-alignment-scoring.md)
