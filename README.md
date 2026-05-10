# BankrSynth Skill for Bankr

Install BankrSynth as a skill in your Bankr agent and get on-demand AI synthesis of Base ecosystem tokens directly in chat.

## What is BankrSynth?

BankrSynth is an intelligence synthesis layer for Base tokens. It pulls live data from DexScreener and CoinGecko (refreshed every 15 minutes) and runs three flavors of AI analysis:

- analyze — deep technical + fundamental breakdown
- narrative — story-driven scan, what's the meta
- thesis — opinionated long/short with conviction

Across seven curated universes:

- all — full Base
- trending_base — 24h movers
- new_launches — last 7 days
- high_volume — top by volume
- ai_agents — Base AI agent tokens
- base_eco — core Base ecosystem
- bankr_eco — $BNKR-adjacent

App lives at:
https://bankr.bot/u/0xa8682fc423b9aa04bd11e76ab01fba5bd2d5c91a/apps/bankrsynth

## Installation

In any Bankr chat (terminal, telegram, or web), say:

```bash
install skill https://github.com/bankrsynth/bankrsynth-skill

Bankr will fetch SKILL.md from this repo and register it. After install, the skill activates automatically when your messages match its triggers.

Usage

Natural language — no slash commands needed:

"synth me trending base"

"bankrsynth thesis on ai_agents"

"give me a narrative scan of new launches"

"analyze bankr eco, top 20"


The skill maps your phrasing to mode + bucket + limit and calls the BankrSynth x402 endpoint.

Pricing

Each call hits a paid x402 endpoint capped at $0.15 USDC. Bankr handles payment automatically using your wallet's USDC balance on Base. You'll see the actual cost in the tool result.

Triggers

bankrsynth

synth me

synthesize base tokens

base token thesis

base narrative scan

analyze base bucket

bankr eco synthesis

ai agent token thesis


Modes

mode	use when

analyze	want metrics, ratios, supply/holder breakdowns
narrative	want the story, what's the meta this week
thesis	want a directional take with conviction scoring


Buckets

bucket	contents

all	full Base universe (filtered by liquidity floor)
trending_base	top 24h price movers
new_launches	tokens launched in last 7d
high_volume	top by 24h volume
ai_agents	AI agent / agentic-economy tokens on Base
base_eco	core Base ecosystem (CB-adjacent, infra, blue chips)
bankr_eco	$BNKR-adjacent tokens


Output shape

JSON returned by the endpoint:

{
  "mode": "thesis",
  "bucket": "ai_agents",
  "generated_at": "ISO-8601 timestamp",
  "tokens": [{ "symbol", "address", "price", "vol24h", "score" }],
  "synthesis": "markdown-formatted long-form analysis",
  "top_picks": ["SYM1", "SYM2", "SYM3"]
}

Bankr renders the synthesis verbatim, lists top picks, and offers to swap if you reference a symbol.

FAQ

Q: Is this on-chain?

A: The data sources are on-chain (DexScreener) and off-chain (CoinGecko). The synthesis is AI-generated. Swap follow-ups are on-chain.

Q: What chains?

A: Base only. For other chains use other tools.

Q: How fresh is the data?

A: The universe refreshes every 15 minutes via cron.

Q: Can I disable the skill?

A: Yes — uninstall_skill("bankrsynth") or /skills in chat.

Author

Built by 0xa8682fc423b9aa04bd11e76ab01fba5bd2d5c91a.

App source on Bankr. Endpoint hosted via Bankr's x402 infrastructure.

License

MIT — see LICENSE.
