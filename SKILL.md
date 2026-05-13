---
name: bankrsynth
version: 1.1.0
author: 0xa8682fc423b9aa04bd11e76ab01fba5bd2d5c91a
description: >
  BankrSynth — AI intelligence synthesis for Base ecosystem tokens. Invoke when user says:
  "bankrsynth", "synth me", "synth base", "base thesis", "base narrative", "analyze base tokens",
  "token scan", "what's hot on base", "base token report", "ai agent thesis", "new launches on base",
  "bankr eco thesis", "high volume base", "trending base tokens", "give me a synthesis",
  "base intelligence", "run a scan", "bankrsynth analyze", "bankrsynth narrative", "bankrsynth thesis".
  Three modes (analyze / narrative / thesis) across seven token universes. Paid x402 endpoint, ≤ $0.15 USDC/call.
tags: [base, research, ai, x402, synthesis, tokens, dex, defi]
---

# BankrSynth Skill

## ⛔ CRITICAL RULES — READ FIRST

1. **Never fabricate token data.** If the endpoint fails, surface the error. Do not invent prices, volumes, or picks.
2. **Do not invoke for price lookups.** Use `get_token_price` instead.
3. **Do not invoke for swaps/buys.** Use swap tools directly. BankrSynth is analysis-only.
4. **Base only.** Other chains → tell user this skill doesn't cover them.
5. **Cost disclosure required before every call.** Say: *"Pulling from a paid x402 endpoint (≤ $0.15 USDC). Proceeding."* Do not block on manual confirmation — `call_x402_endpoint` handles payment natively.

---

## Parameter Extraction

| param | required | default | values |
|-------|----------|---------|--------|
| `mode` | yes | `thesis` | `analyze` · `narrative` · `thesis` |
| `bucket` | yes | `trending_base` | `all` · `trending_base` · `new_launches` · `high_volume` · `ai_agents` · `base_eco` · `bankr_eco` |
| `limit` | no | `15` | integer 5–50 |

→ See `references/modes.md` for when to pick each mode.  
→ See `references/buckets.md` for bucket contents and disambiguation.

---

## Execution

**Step 1 — Disclose cost**
> "Pulling from a paid x402 endpoint (≤ $0.15 USDC). Proceeding."

**Step 2 — Call endpoint**
```
tool: call_x402_endpoint
url:  https://x402.bankr.bot/0xa8682fc423b9aa04bd11e76ab01fba5bd2d5c91a/bankrsynth-ai
method: POST
body: { "mode": "<mode>", "bucket": "<bucket>", "limit": <limit> }
maxCostUsdc: 0.15
```
If `call_x402_endpoint` is not bound: `request_additional_tools({ names: ["call_x402_endpoint"] })` first.

**Step 3 — Render response**
→ See `references/output.md` for JSON shape and rendering rules.

**Step 4 — Optional swap handoff**
If user says *"swap into TOKEN"* or *"ape TOKEN"*, hand off to standard swap flow using the `address` from the `tokens` array. Confirm amount + symbol before submitting.

---

## Error Handling

| error | action |
|-------|--------|
| 402 / payment rejected | Tell user, ask to retry with explicit confirmation |
| 5xx / timeout | Retry once after 3s; surface error if still failing |
| empty `tokens` array | "No qualifying tokens in this bucket right now" + suggest alternate bucket |
| malformed JSON | Surface raw error, do not guess at content |

## When to invoke

Invoke this skill whenever the user asks for:
- a "BankrSynth" report, scan, or thesis
- AI analysis, narrative, or thesis on Base tokens
- a synthesis of trending / new / high-volume / AI-agent / Base-eco / Bankr-eco tokens
- "synth me <bucket>" or "give me a thesis on <bucket>" patterns

Do NOT invoke for:
- generic price lookups (use get_token_price)
- buying/selling tokens (use the swap tools directly)
- non-Base chains

## Parameters to extract from user message

1. mode (required) — one of:
   - analyze: deep technical/fundamental breakdown of a single bucket
   - narrative: short narrative-driven scan, story-first framing
   - thesis: opinionated long/short thesis with conviction scoring
   - default if ambiguous: thesis
2. bucket (required) — one of:
   - all: full Base universe
   - trending_base: hottest movers last 24h
   - new_launches: tokens launched in last 7d
   - high_volume: top by 24h volume
   - ai_agents: AI agent tokens on Base
   - base_eco: core Base ecosystem
   - bankr_eco: Bankr/$BNKR-adjacent tokens
   - default if ambiguous: trending_base
3. limit (optional, integer 5–50, default 15) — how many tokens to include in synthesis

## Execution steps

Step 1 — Confirm cost
The endpoint is paid (x402, max $0.15 USDC per call). Before calling, tell the user:
"This pulls from a paid x402 endpoint (≤ $0.15 USDC). Proceeding."
If user has not pre-approved x402 spend, the call_x402_endpoint tool will prompt natively — do not block on a manual confirmation.

Step 2 — Call the endpoint
Use call_x402_endpoint with:
- url: https://x402.bankr.bot/0xa8682fc423b9aa04bd11e76ab01fba5bd2d5c91a/bankrsynth-ai
- method: POST
- body: { "mode": "<mode>", "bucket": "<bucket>", "limit": <limit> }
- maxCostUsdc: 0.15

If call_x402_endpoint is not bound, request_additional_tools({ names: ["call_x402_endpoint"] }) first.

Step 3 — Format the response
The endpoint returns JSON like:
{
  "mode": "thesis",
  "bucket": "ai_agents",
  "generated_at": "2026-05-09T...",
  "tokens": [ { "symbol": "...", "address": "0x...", "price": ..., "vol24h": ..., "score": ... } ],
  "synthesis": "long-form markdown text",
  "top_picks": ["TOKEN1", "TOKEN2", "TOKEN3"]
}

Render to user as:
- Header: "BankrSynth · <mode> · <bucket>"
- Body: the synthesis field verbatim (it is already formatted)
- Footer: "Top picks: <top_picks joined by · >"
- Add: "Powered by BankrSynth — bankr.bot/u/0xa8682fc423b9aa04bd11e76ab01fba5bd2d5c91a/apps/bankrsynth"

Step 4 — Optional follow-up
If user says "swap into <symbol>" or "ape <symbol>" referencing a token from the response, hand off to the standard swap flow with the token address from the tokens array. Confirm amount + token before submitting.

## Error handling

- 402 / payment required → tell user x402 spend was rejected, ask to retry with confirmation
- 5xx / timeout → retry once after 3s; if still failing, surface the error and stop
- empty tokens array → tell user "bucket has no qualifying tokens right now" and suggest a different bucket
- never fabricate token data if endpoint fails

## Examples (for agent priming)

User: "synth me ai agents"
→ mode=thesis, bucket=ai_agents, limit=15

User: "give me a narrative scan on new Base launches, top 25"
→ mode=narrative, bucket=new_launches, limit=25

User: "bankrsynth analyze bankr eco"
→ mode=analyze, bucket=bankr_eco, limit=15

User: "what's hot on base rn, bankrsynth thesis"
→ mode=thesis, bucket=trending_base, limit=15
