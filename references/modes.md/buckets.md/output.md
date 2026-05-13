# BankrSynth — Output Reference

## Response JSON Shape

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
  "synthesis": "long-form markdown text — render verbatim",
  "top_picks": ["SYM1", "SYM2", "SYM3"]
}
```

## Rendering Rules

**Header line:**
```
BankrSynth · <MODE> · <BUCKET> · <generated_at as human time>
```

**Body:**
Render `synthesis` verbatim — it is already markdown-formatted. Do not summarize or paraphrase.

**Footer:**
```
Top picks: SYM1 · SYM2 · SYM3
Powered by BankrSynth — bankr.bot/u/0xa8682fc423b9aa04bd11e76ab01fba5bd2d5c91a/apps/bankrsynth
```

**Actual cost:**
If the tool result includes a `cost_usdc` field, append: `Cost: $X.XX USDC`

## Swap Handoff

After rendering, if user references a symbol from `top_picks` or `tokens`:
- Extract the matching `address` from `tokens` array
- Hand off to standard swap flow
- Confirm: "Swap [AMOUNT] USDC into [SYMBOL] at [ADDRESS]?" before submitting
- Never assume amount — always ask if not specified

## Do Not

- Do not summarize or shorten the `synthesis` field
- Do not invent `top_picks` if the field is missing — skip the footer picks line instead
- Do not display raw token addresses in the main response unless user asks

