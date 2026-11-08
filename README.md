# Oracle by SaltCrate

**Fiat-settled pay-per-call web intelligence** for AI agents — live search, schema-driven structured extraction, clean page fetch, link checking, and recurring change monitoring. No API key, no subscription, no crypto wallet required.

- **Endpoint (Streamable HTTP):** `https://oracle.saltcrate.com/mcp`
- **Pricing:** $1.00/tool call via Stripe MPP card, or $0.05/call in USDC (Tempo) for high-volume search/extract.
- **Payments:** Stripe Machine Payments (card/SPT) + Tempo USDC — an unpaid call returns a `-32042` "Payment Required" challenge; the agent pays, retries, and gets the result + receipt.

## Tools

| Tool | What it does |
|---|---|
| `fetch` | Clean web extraction — boilerplate, navigation, and ads stripped |
| `extract` | Schema-driven structured extraction into typed JSON (DeepSeek) |
| `search` | Live web search — title, URL, snippet (DuckDuckGo / Brave / Serper) |
| `check_link` | URL liveness — HTTP status, redirect chain, final URL, latency |
| `check_change` | Hash-based page-change detection |
| `monitor` | Recurring URL watch — sticky change tracking with timestamps |
| `verify` | Heuristic claim-vs-source check — confidence score + excerpt |

This repo hosts the LobeHub plugin manifest (`lhm.plugin.json`) pointing at the hosted MCP endpoint.
