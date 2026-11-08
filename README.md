# Oracle by SaltCrate

**Live web intelligence for AI agents — pay per call, no subscription.** Seven MCP tools:
live search, schema-driven structured extraction, clean page fetch, link checking, change
detection, recurring monitoring, and claim verification.

- **Endpoint (Streamable HTTP):** `https://oracle.saltcrate.com/mcp`
- **Pricing:** $0.01/call in USDC (Tempo) on every tool · $0.50/call by card on `extract` + `monitor`
- **No wallet?** **Prepaid access pass: $5 for 1,000 calls** → <https://oracle.saltcrate.com/buy>

## Quickstart (60 seconds, card, no wallet)

1. Buy a pass: <https://oracle.saltcrate.com/buy> ($5 → 1,000 calls, valid 12 months)
2. Copy the `orc_…` key it gives you.
3. Call any tool with the key as a bearer token:

```bash
curl -s https://oracle.saltcrate.com/mcp \
  -H "Authorization: Bearer orc_your_key_here" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json, text/event-stream" \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/call",
       "params":{"name":"search","arguments":{"query":"silver spot price"}}}'
```

Check remaining calls: `GET https://oracle.saltcrate.com/usage?key=orc_…`

## Pay-per-call (no account)

An unpaid call returns a `-32042` "Payment Required" challenge carrying both rails — USDC
via Tempo and, on `extract`/`monitor`, a Stripe card charge. The agent pays, retries, and
gets the result plus a receipt. Nothing is stored: one charge, one call.

## Tools

| Tool | What it does | USDC | Card |
|---|---|---|---|
| `fetch` | Clean extraction — boilerplate, nav, ads stripped | $0.01 | — |
| `search` | Live web search — title, URL, snippet (DuckDuckGo / Brave / Serper) | $0.01 | — |
| `extract` | Schema-driven structured extraction into typed JSON (DeepSeek) | $0.01 | $0.50 |
| `check_link` | URL liveness — status, redirect chain, final URL, latency | $0.01 | — |
| `check_change` | Hash-based page-change detection | $0.01 | — |
| `monitor` | Recurring URL watch — sticky change tracking with timestamps | $0.01 | $0.50 |
| `verify` | Heuristic claim-vs-source check — confidence + supporting excerpt | $0.01 | — |

Discovery files: [`/llms.txt`](https://oracle.saltcrate.com/llms.txt) ·
[`/openapi.json`](https://oracle.saltcrate.com/openapi.json) ·
[`/pricing.md`](https://oracle.saltcrate.com/pricing.md)

Self-host (free variant): `python server.py`. Paid server: `server_paid.py` (see its module
docstring for env vars). Deployable anywhere Docker runs (`Dockerfile` + `fly.toml` included).

A [SaltCrate](https://saltcrate.com) product.
