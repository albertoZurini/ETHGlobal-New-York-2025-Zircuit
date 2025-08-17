# ETHGlobal-New-Tork-2025 ToolBooth


# ToolBooth: Zircuit-Native AI-Secured Micropaywalls

Short description:
ToolBooth is an AI-hardened micropaywall protocol that lets websites and publishers charge small fees (via HTTP 402) for bot or agent access. By building on Zircuit, ToolBooth leverages Sequencer-Level Security (SLS) and Account Abstraction (AA) to deliver safer, fraud-resistant payments at internet scale.

Why we should win:
All major protocols are converging on x402-style pay-per-access. Zircuit can own this narrative by offering the only version hardened at the sequencer level. ToolBooth creates FOMO: if publishers integrate x402 elsewhere, they’ll miss the unique protections that only Zircuit provides.

---

## Problem

Captchas frustrate users and are ineffective against agents. As agents start to mediate more of the actions on the web, publishers face a binary choice: block all automation or let themselves be botted/farmed. With the rise of agentic browsing, web scraping, and AI traffic, this problem grows daily.

Existing pay-per-crawl/paywall models (like x402) show market demand but suffer from:

* Replay/fraud risk: repeated access with one payment.
* Abuse: coordinated toll-evasion, proxy attacks.
* UX friction: clunky wallet approvals, manual signing.

---

## Solution

ToolBooth introduces a Zircuit-native paywall protocol:

1. HTTP 402 Tolling: Sites respond with 402 Payment Required and a cost header.
2. Facilitator Service: An agent or user hits the ToolBooth facilitator, which:

   * uses AA session wallets (EIP-7702) to auto-pay fees with strict spend limits,
   * settles transactions on Zircuit,
   * mints an Access Pass (signed token with expiry & nonce).
3. Publisher Middleware: Verifies Access Pass before serving content.
4. Revenue Splits: Built-in N-way payouts for publishers, journalists, contributors.

---

## Why Zircuit

ToolBooth can run on any EVM chain, but it’s a materially better fit on Zircuit because the protocol lines up with what a 402 micropaywall actually needs.

Sequencer-level security stops abuse at the source. Most micropaywalls die from replay and “pay-once, reuse everywhere” attacks. Zircuit screens transactions before they ever land in a block, so patterns like nonce trees, coordinated session churn, and obvious toll-evasion get blocked upstream instead of forcing publishers to play whack-a-mole at the app layer.

ZK batching keeps receipts cheap and sane. A paywall generates tons of tiny payments. Zircuit’s rollup flow lets us batch those into a single settlement per epoch with a Merkle root of paid accesses. Publishers verify inclusion off-chain at line speed and still get on-chain finality—no chain bloat, no death by gas.

Account abstraction makes the UX invisible. Session wallets (7702-style) give agents per-domain spend caps, TTLs, and gas sponsorship. Translation: no wallet pop-ups, no manual signing, and safe delegation for users. That’s the difference between “nice demo” and something agents can actually run 24/7.

Funding is easy, wherever the money sits. Zircuit plays well with common bridges, so facilitators can top up on predictable schedules. Users fund once from the chain they live on; ToolBooth handles the rest. Less friction means more paid requests.

Boring, compatible tooling. It’s EVM. Contracts and middleware deploy with standard stacks, verification lives at the edge, and none of this forces publishers into weird infra or on-chain content schemes.

Right place, right time. x402 is consolidating into a real pattern for “pay for access.” Zircuit adds two things others don’t at the protocol layer: pre-inclusion screening and cheap proof batching. That’s exactly what turns 402 from a slide into a business.

Concrete “Zircuit-only” hooks we’d use

Sequencer policies for passes: Rate-limit repeated jti/nonce trees and flag correlated session churn across domains so bad traffic never settles.

Receipt trees + epoch settlements: One root per epoch instead of thousands of micro-txs; instant off-chain verification, on-chain finality when you need it.

Default to session wallets: Per-route price caps, TTLs, and sponsor gas to keep agent flows uninterrupted and safe.

Bridged balances: Facilitator keeps a Zircuit float and refills from other chains on a timer—no per-request bridging nonsense.
---

## Architecture Overview
[Publisher Site] --402--> [ToolBooth Facilitator] --tx--> [Zircuit TollBooth Contract]
      ^                        |                               |
      |<-- Access Pass --------|<-- Signed JWT/Nonce ----------|
* Publisher Site: responds with HTTP 402 challenge.
* Facilitator: pays fee on Zircuit via session wallet, mints Access Pass.
* Zircuit TollBooth Contract: verifies payment, manages expiry & splits.
* Access Pass: bearer token sent back to site; validated by middleware.

---

## User Journeys

Agent / User Flow

1. User/agent requests content → gets 402.
2. Facilitator auto-pays on Zircuit via AA session wallet.
3. Contract issues Access Pass (time-boxed).
4. Agent retries request → publisher validates pass → content unlocked.

Publisher Flow

1. Set price, expiry, revenue splits via config dashboard.
2. Drop middleware snippet in site backend.
3. Monitor receipts & revocations.

---

## Roadmap

* Milestone 0 (Hackathon Submission)

  * Problem & solution statement
  * Architecture diagram
  * User journeys
  * Demo site with 402 middleware
  * Why Zircuit: SLS + AA advantage
  * Repo with README + optional diagram

* Milestone 1 (Spec & Stubs)

  * Solidity interface for TollBooth contract
  * Facilitator API schema
  * Session wallet config draft

* Milestone 2 (PoC on Garfield)

  * Deploy contracts to Garfield Testnet
  * Verify on Zircuit Explorer

* Milestone 3 (Publisher UX)

  * Dashboard for pricing/splits
  * Receipt & revocation view

---
