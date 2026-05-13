# BankrSynth — Buckets Reference

## Bucket Contents

| bucket | contents | refresh |
|--------|----------|---------|
| `all` | Full Base universe, filtered by liquidity floor | 15 min |
| `trending_base` | Top 24h price movers on Base | 15 min |
| `new_launches` | Tokens launched in last 7 days | 15 min |
| `high_volume` | Top tokens by 24h USD volume | 15 min |
| `ai_agents` | AI agent / agentic-economy tokens on Base | 15 min |
| `base_eco` | Core Base ecosystem (CB-adjacent, infra, blue chips) | 15 min |
| `bankr_eco` | $BNKR-adjacent tokens | 15 min |

## Disambiguation

- "trending" / "hot" / "movers" → `trending_base`
- "new" / "launches" / "fresh" / "just launched" / "last week" → `new_launches`
- "volume" / "most traded" → `high_volume`
- "ai" / "agents" / "agentic" / "AI tokens" → `ai_agents`
- "base ecosystem" / "base blue chips" / "cb" / "coinbase" → `base_eco`
- "bankr" / "bnkr" / "bankr eco" → `bankr_eco`
- "everything" / "all base" / no bucket hint → `all` (warn user this is a larger, slower call)
- Ambiguous with no keyword → default `trending_base`

## Cost Note

Higher `limit` = more tokens processed = higher synthesis cost (still capped at $0.15).
For `all` bucket, recommend `limit` ≤ 20 unless user explicitly wants more.

