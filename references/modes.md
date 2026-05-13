# BankrSynth — Modes Reference

## Mode Selection Guide

| mode | use when user says | output character |
|------|--------------------|-----------------|
| `analyze` | "analyze", "deep dive", "breakdown", "metrics", "fundamentals", "supply", "holders", "ratios" | Technical. Metrics-heavy. Supply/holder breakdown, volume ratios, liquidity depth. |
| `narrative` | "narrative", "story", "meta", "what's the vibe", "what's trending", "cultural angle" | Story-first. Short-form. What's the meta this week, which tokens have momentum narratives. |
| `thesis` | "thesis", "conviction", "long", "short", "pick", "directional", "top picks", "what should I ape" | Opinionated. Conviction-scored. Long/short directional takes on each token. |

## Ambiguity Resolution

- User says "scan" or "report" with no mode hint → default `thesis`
- User says "what's hot" / "what's moving" → `thesis` on `trending_base`
- User says "tell me about new tokens" → `narrative` on `new_launches`
- User asks for "metrics" or "data" → `analyze`
- User explicitly names a mode → always honor it

