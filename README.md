# ToolBooth
## Zircuit-Native AI-Secured Micropaywalls

> **ETHGlobal New York 2025 Submission**

### Executive Summary

**ToolBooth** is an AI-hardened micropaywall protocol that enables websites and publishers to charge small fees (via HTTP 402) for bot or agent access. Built on Zircuit, ToolBooth leverages **Sequencer-Level Security (SLS)** and **Account Abstraction (AA)** to deliver safer, fraud-resistant payments at internet scale.

### Why We Should Win

All major protocols are converging on **x402-style pay-per-access**. Zircuit can own this narrative by offering the only version hardened at the sequencer level. ToolBooth creates FOMO: if publishers integrate x402 elsewhere, they'll miss the unique protections that only Zircuit provides.

---

## Problem Statement

**Current State:** Captchas frustrate users and are ineffective against agents. As agents start to mediate more web actions, publishers face a binary choice:
- Block all automation
- Let themselves be botted/farmed

With the rise of agentic browsing, web scraping, and AI traffic, this problem grows daily.

### Issues with Existing Solutions

Current pay-per-crawl/paywall models (like x402) show market demand but suffer from:

| Issue | Impact |
|-------|--------|
| **Replay/fraud risk** | Repeated access with one payment |
| **Coordinated abuse** | Toll-evasion, proxy attacks |
| **UX friction** | Clunky wallet approvals, manual signing |

---

## Solution: ToolBooth Protocol

ToolBooth introduces a **Zircuit-native paywall protocol** with four key components:

### Flow Overview

1. **HTTP 402 Tolling**
   - Sites respond with `402 Payment Required` and a cost header

2. **Facilitator Service** 
   - Agent/user hits ToolBooth facilitator, which:
     - Uses AA session wallets (EIP-7702) for auto-pay with strict spend limits
     - Settles transactions on Zircuit
     - Mints an Access Pass (signed token with expiry & nonce)

3. **Publisher Middleware**
   - Verifies Access Pass before serving content

4. **Revenue Splits**
   - Built-in N-way payouts for publishers, journalists, contributors

---

## Why Zircuit?

> ToolBooth can run on any EVM chain, but it's a **materially better fit** on Zircuit because the protocol aligns with what a 402 micropaywall actually needs.

### Sequencer-Level Security
- **Problem:** Most micropaywalls die from replay and "pay-once, reuse everywhere" attacks
- **Zircuit Solution:** Screens transactions before they hit blocks, stopping nonce trees, coordinated session churn, and toll-evasion upstream

### ZK Batching
- **Problem:** Paywalls generate tons of tiny payments
- **Zircuit Solution:** Batch payments into single settlement per epoch with Merkle root of paid accesses
- **Result:** Publishers verify inclusion off-chain at line speed with on-chain finality

### Account Abstraction
- **Problem:** Wallet pop-ups kill user experience
- **Zircuit Solution:** Session wallets (7702-style) with per-domain spend caps, TTLs, and gas sponsorship
- **Result:** No wallet pop-ups, no manual signing, safe 24/7 agent operation

### Seamless Funding
- **Benefit:** Compatible bridges, predictable top-ups, fund once from any chain
- **Result:** Less friction = more paid requests

### Developer-Friendly
- **Standard EVM tooling**
- **Edge verification**
- **No weird infrastructure requirements**

### Market Timing
- **x402 consolidating into real pattern**
- **Zircuit adds unique protocol-layer features:**
  - Pre-inclusion screening
  - Cheap proof batching

---

## Architecture

```
[Publisher Site] --402--> [ToolBooth Facilitator] --tx--> [Zircuit TollBooth Contract]
      ^                        |                               |
      |<-- Access Pass --------|<-- Signed JWT/Nonce ----------|
```

### Components

| Component | Function |
|-----------|----------|
| **Publisher Site** | Responds with HTTP 402 challenge |
| **Facilitator** | Pays fee on Zircuit via session wallet, mints Access Pass |
| **Zircuit TollBooth Contract** | Verifies payment, manages expiry & splits |
| **Access Pass** | Bearer token sent back to site; validated by middleware |

---

## User Journeys

### Agent/User Flow

1. **Request** → User/agent requests content → gets 402
2. **Payment** → Facilitator auto-pays on Zircuit via AA session wallet
3. **Authorization** → Contract issues Access Pass (time-boxed)
4. **Access** → Agent retries request → publisher validates pass → content unlocked

### Publisher Flow

1. **Setup** → Set price, expiry, revenue splits via config dashboard
2. **Integration** → Drop middleware snippet in site backend
3. **Monitor** → Track receipts & revocations

---

## Roadmap

### Milestone 0: Hackathon Submission ✅
- [x] Problem & solution statement
- [x] Architecture diagram  
- [x] User journeys
- [x] Demo site with 402 middleware
- [x] Zircuit advantage analysis
- [x] Repository with documentation

### Milestone 1: Spec & Stubs
- [ ] Solidity interface for TollBooth contract
- [ ] Facilitator API schema
- [ ] Session wallet configuration draft

### Milestone 2: PoC on Garfield

**Core Development:**
- [ ] Minimal contract recording receipts with basic splits
- [ ] Facilitator with payment processing and pass issuance
- [ ] Session wallets with caps/TTL and gas sponsorship
- [ ] Middleware with signature/expiry/domain verification

**Demo Implementation:**
- [ ] Demo site with per-request and per-KB routes
- [ ] "Revoke pass" functionality
- [ ] Performance metrics collection

**Success Criteria:**
- ✅ Two paid unlocks succeed
- ✅ Replay attack fails
- ✅ Revoked pass blocked
- ✅ Edge verification adds ≤80ms p99
- ✅ Batched cost per request <$0.002

### Milestone 3: Publisher UX
- [ ] **Dashboard Features:**
  - Edit prices & caps
  - Manage revenue splits
  - View receipts
  - Revoke passes
  - Export CSV reports

---

## Zircuit-Specific Features

### Sequencer Policies
- Rate-limit repeated jti/nonce trees
- Flag correlated session churn across domains
- Block bad traffic before settlement

### Receipt Trees + Epoch Settlements
- One root per epoch instead of thousands of micro-transactions
- Instant off-chain verification
- On-chain finality when needed

### Default Session Wallets
- Per-route price caps
- Time-to-live limits
- Sponsored gas for uninterrupted agent flows

### Bridged Balances
- Facilitator maintains Zircuit float
- Timer-based refills from other chains
- No per-request bridging overhead

---

*Built for ETHGlobal New York 2025*
