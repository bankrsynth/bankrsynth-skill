# BankrSynth Skill

> AI-powered intelligence synthesis for Base ecosystem tokens — installed directly into your Bankr agent.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Bankr Skill](https://img.shields.io/badge/Bankr-Skill-8b5cf6)](https://bankr.bot/u/0xa8682fc423b9aa04bd11e76ab01fba5bd2d5c91a/apps/bankrsynth)
[![Chain: Base](https://img.shields.io/badge/Chain-Base-0052ff)](https://base.org)
[![x402](https://img.shields.io/badge/Paid-x402-green)](https://x402.org)

---

## What is BankrSynth?

BankrSynth is an intelligence synthesis layer purpose-built for Base ecosystem tokens. Instead of showing you raw price data, it pulls live market data from DexScreener and CoinGecko — refreshed every 15 minutes — and runs AI-generated analysis across three distinct modes.

The result lands directly in your Bankr chat as structured, readable intelligence. No dashboards to open, no tabs to switch. Just ask in plain English and get a synthesis back.

**Live app:** https://bankr.bot/u/0xa8682fc423b9aa04bd11e76ab01fba5bd2d5c91a/apps/bankrsynth

---

## How It Works

```
User message in Bankr chat
        ↓
Skill detects trigger phrase
        ↓
Extracts: mode + bucket + limit
        ↓
Calls x402 endpoint (≤ $0.15 USDC, paid from your Base wallet)
        ↓
Endpoint queries DexScreener + CoinGecko → runs AI synthesis
        ↓
Returns: synthesis text + top picks + token list
        ↓
Bankr renders result in chat — offers swap if you name a token
```

---

## Installation

In any Bankr chat (terminal, Telegram, or web):

```
install skill https://github.com/bankrsynth/bankrsynth-skill
```

Bankr fetches `SKILL.md` from this repo, registers the skill, and activates it automatically. No config needed. The skill fires whenever your message matches a trigger phrase.

To remove it:

```
uninstall_skill("bankrsynth")
```

Or navigate to `/skills` in the Bankr chat interface and disable from there.

---

## Usage

Natural language only — no slash commands, no special syntax:

```
"synth me trending base"
"bankrsynth thesis on ai_agents"
"give me a narrative scan of new launches, top 25"
"analyze bankr eco"
"what's hot on base rn"
"base token report, high volume"
"run a synthesis on new Base launches"
"give me a thesis on ai agent tokens, top 20"
```

The skill parses your phrasing and maps it to three parameters: **mode**, **bucket**, and **limit**.

---

## Modes

| Mode | What it produces | Use when you want |
|------|-----------------|-------------------|
| `analyze` | Deep technical and fundamental breakdown — price ratios, supply distribution, holder concentration, volume/liquidity metrics | Raw data, numbers, on-chain metrics |
| `narrative` | Short-form, story-driven scan focused on momentum narratives and cultural meta | The vibe, what's trending, what story the market is telling |
| `thesis` | Opinionated long/short directional takes with conviction scores per token | Actionable picks, a clear point of view |

**Default if unspecified:** `thesis`

---

## Buckets

Seven curated token universes, each filtered and refreshed every 15 minutes:

| Bucket | Contents | Best for |
|--------|----------|----------|
| `all` | Full Base universe, filtered by liquidity floor | Broad market overview |
| `trending_base` | Top price movers over the last 24h | What's pumping right now |
| `new_launches` | Tokens launched in the last 7 days | Early discovery, fresh gems |
| `high_volume` | Top tokens ranked by 24h USD volume | Where capital is actually moving |
| `ai_agents` | AI agent and agentic-economy tokens on Base | The AI narrative on Base |
| `base_eco` | Core Base ecosystem — CB-adjacent, infrastructure, blue chips | Established Base plays |
| `bankr_eco` | $BNKR-adjacent tokens | Bankr community tokens |

**Default if unspecified:** `trending_base`

**Limit** controls how many tokens get included in the synthesis. Range: 5–50. Default: 15. Higher limits = richer synthesis, but the cost stays capped at $0.15 USDC either way.

---

## Pricing

Each call hits a paid [x402](https://x402.org) endpoint.

- **Max cost:** $0.15 USDC per call
- **Payment:** Automatic from your USDC balance on Base — Bankr handles the payment natively
- **Actual cost:** Shown in the tool result after each call
- **No subscription:** Pay per synthesis, nothing upfront

The skill will disclose the cost before calling and proceed automatically. If your wallet lacks sufficient USDC on Base, the call will fail with a 402 error.

---

## Output

After synthesis, Bankr renders the result as:

```
BankrSynth · <mode> · <bucket> · <timestamp>

<long-form AI synthesis — formatted markdown>

Top picks: TOKEN1 · TOKEN2 · TOKEN3
Powered by BankrSynth — bankr.bot/u/0xa8682...
```

If you follow up with `"swap into TOKEN1"` or `"ape TOKEN2"`, Bankr hands off to its standard swap flow using the token's on-chain address from the synthesis result. You'll be asked to confirm the amount before anything executes.

**Raw JSON shape returned by the endpoint:**

```json
{
  "mode": "thesis",
  "bucket": "ai_agents",
  "generated_at": "2026-05-14T12:00:00Z",
  "tokens": [
    {
      "symbol": "TOKEN",
      "address": "0x...",
      "price": 0.0042,
      "vol24h": 180000,
      "score": 82
    }
  ],
  "synthesis": "markdown-formatted long-form analysis",
  "top_picks": ["SYM1", "SYM2", "SYM3"]
}
```

---

## Repo Structure

```
bankrsynth-skill/
├── SKILL.md                  # Skill definition — loaded by Bankr agent
├── references/
│   ├── modes.md              # Mode selection logic and disambiguation
│   ├── buckets.md            # Bucket contents, keywords, cost notes
│   └── output.md             # JSON shape, rendering rules, swap handoff
├── example.md                # Annotated example interactions
├── LICENSE
└── README.md
```

`SKILL.md` is the only file Bankr strictly requires. The `references/` files provide deeper context that the agent loads on demand — keeping the core skill context tight and focused.

---

## Trigger Phrases

The skill activates on any of these patterns (and natural variations):

```
bankrsynth
synth me
synthesize base tokens
base token thesis
base narrative scan
base token report
analyze base tokens
analyze base bucket
bankr eco synthesis
ai agent token thesis
what's hot on base
give me a synthesis
run a scan
base intelligence
trending base tokens
new launches on base
```

---

## FAQ

**Is this on-chain?**
Data sourcing is hybrid: DexScreener (on-chain DEX data) and CoinGecko (off-chain aggregated). The synthesis layer is AI-generated. Swap follow-ups — if you choose to act on a pick — are fully on-chain.

**Base only?**
Yes. BankrSynth is built specifically for the Base ecosystem. For other chains, use other tools.

**How fresh is the data?**
The token universe refreshes every 15 minutes via a background cron. The synthesis is generated fresh on each call using the latest available snapshot.

**What if the endpoint is down?**
The skill retries once after 3 seconds. If it still fails, it surfaces the error rather than fabricating data. Token data is never invented.

**What if a bucket has no qualifying tokens?**
The skill will tell you the bucket returned no results and suggest trying a different one.

**Can I run multiple modes on the same bucket?**
Yes — just make separate requests. Each call is independent.

---

## Author

Built by [`0xa8682fc423b9aa04bd11e76ab01fba5bd2d5c91a`](https://bankr.bot/u/0xa8682fc423b9aa04bd11e76ab01fba5bd2d5c91a).

App source and endpoint hosted via Bankr's x402 infrastructure.

---

## License

MIT — see [LICENSE](LICENSE).
