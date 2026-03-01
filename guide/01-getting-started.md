# 01 - Getting Started: Account Setup & Wallet Activation

## Overview

Before you can earn PFT, you need three things:
1. A Task Node account (authenticated via X/Twitter or GitHub)
2. An activated XRPL wallet on the PFTL testnet
3. The `pft-test-client` CLI installed and configured

This section walks through each step with expected outputs so you know you're on the right track.

---

## Step 1: Sign Up on Task Node

1. Go to [https://tasknode.postfiat.org](https://tasknode.postfiat.org)
2. Click **Sign Up** and authenticate with either:
   - **X (Twitter)** -- uses your X handle as your operator identity
   - **GitHub** -- uses your GitHub username as your operator identity
3. Once authenticated, you'll land on the Task Node dashboard

Your identity on the network is tied to your auth provider. If you sign up with GitHub as `your-username`, your operator identity across the network is `your-username`.

> **Tip:** Use the same identity you want associated with your work. Your X handle or GitHub username will appear on the leaderboard and in task attributions.

## Step 2: Wallet Creation and Activation

After signing up, Task Node creates an XRPL wallet for you on the PFTL testnet.

- Your wallet address looks like: `rXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX`
- You'll receive a **12-word mnemonic phrase** -- store this securely
- The wallet is activated automatically with a small PFTL balance for transaction fees

**Important:** Your mnemonic is the only way to recover your wallet. Write it down physically and store it somewhere safe. If you lose it, your PFT balance and task history are unrecoverable.

Your wallet address is visible on your Task Node profile page. You can also find it in the leaderboard at [https://tasknode.postfiat.org](https://tasknode.postfiat.org).

## Step 3: Install the pft-test-client CLI

The CLI is your primary interface for requesting tasks, submitting evidence, and responding to verification questions.

```bash
# Clone the repository
git clone https://github.com/postfiatorg/pft-test-client.git

# Navigate to the TypeScript client
cd pft-test-client/ts

# Install dependencies
npm install

# Build the project
npm run build
```

Expected output after `npm run build`:
```
> pft-test-client@1.0.0 build
> tsc
```

No errors means you're good. If you see TypeScript errors, make sure you have Node.js 18+ and npm 9+ installed:

```bash
node --version   # should be v18.x or higher
npm --version    # should be 9.x or higher
```

## Step 4: Set Your Credentials

You need two credentials configured:

### JWT Token (from browser)

The JWT authenticates your CLI requests against the Task Node API.

1. Log in to [https://tasknode.postfiat.org](https://tasknode.postfiat.org) in your browser
2. Open DevTools (F12 or Cmd+Shift+I)
3. Go to **Application** > **Local Storage** > `https://tasknode.postfiat.org`
4. Find the JWT token (usually stored under a key like `token` or `jwt`)
5. Copy the full token string

Then set it in the CLI:

```bash
npx pft-cli auth:set-token
# Paste your JWT when prompted
```

This stores the token in `~/.pft-tasknode/config.json`.

> **Note:** JWTs expire. If you get authentication errors after a few hours, repeat this process to refresh your token. See [Common Pitfalls](07-common-pitfalls.md) for more on JWT expiration.

### Wallet Mnemonic (environment variable)

Your wallet mnemonic signs on-chain transactions (task acceptance, evidence submission).

```bash
# Add to your shell profile (~/.zshrc or ~/.bashrc)
export PFT_WALLET_MNEMONIC="word1 word2 word3 word4 word5 word6 word7 word8 word9 word10 word11 word12"
```

Then reload your shell:

```bash
source ~/.zshrc  # or source ~/.bashrc
```

> **Security:** Never commit your mnemonic to git. Never paste it in Discord or any public channel. The environment variable approach keeps it out of config files that might accidentally get shared.

## Step 5: Verify Your Setup

Run the status check:

```bash
npx pft-cli auth:status
```

Expected output (healthy setup):

```
Auth Status:
  JWT Token: Set (expires in 23h 45m)
  Wallet: rYourWalletAddressHere
  Network: PFTL Testnet
  Balance: 50.00 PFTL
  Status: Ready
```

What to look for:
- **JWT Token:** Should say "Set" with remaining time
- **Wallet:** Should show your XRPL address
- **Network:** Should be PFTL Testnet
- **Status:** Should say "Ready"

If you see errors:
- `JWT Token: Not set` -- re-run `auth:set-token`
- `Wallet: Not found` -- check that `PFT_WALLET_MNEMONIC` is set in your environment
- `Network: Unreachable` -- check your internet connection; the PFTL testnet may be temporarily down

## Step 6: Your First Command

Once everything is green, try listing available tasks:

```bash
npx pft-cli tasks:list
```

Or check your current task history:

```bash
npx pft-cli tasks:history
```

You're now ready to move on to writing your [Context Document](02-context-document.md), which is the single biggest lever for getting high-quality tasks assigned to you.

---

## Checklist

- [ ] Signed up on Task Node (X or GitHub auth)
- [ ] Wallet created and mnemonic stored securely
- [ ] `pft-test-client` cloned, installed, and built
- [ ] JWT token set via `auth:set-token`
- [ ] `PFT_WALLET_MNEMONIC` exported in shell profile
- [ ] `auth:status` shows "Ready"
- [ ] First command (`tasks:list` or `tasks:history`) runs without errors

---

Next: [Context Document Best Practices](02-context-document.md)
