# Oracle by SaltCrate

[![saltcrate/saltcrate-oracle MCP server](https://glama.ai/mcp/servers/saltcrate/saltcrate-oracle/badges/score.svg)](https://glama.ai/mcp/servers/saltcrate/saltcrate-oracle) [![SaltCrate Oracle MCP connector](https://glama.ai/mcp/connectors/com.saltcrate/oracle/badges/score.svg)](https://glama.ai/mcp/connectors/com.saltcrate/oracle)

**Verification and freshness for AI agents — free to start, no key, no signup, no wallet.**
Search is the most commoditised slot in the MCP registry (10.9% of 63,678 servers give it away
free); monitoring and change-detection are 1.3% and verification 3.7%. Eight MCP tools: claim
verification, recurring monitoring, hash-based change detection, dependency CVE scanning,
schema-driven structured extraction, clean page fetch, link checking, and live search.

> **The market, measured:** we pulled all 63,678 listings in the official MCP registry — 67.5% are
> hosted remotely and only **0.93% state a price** (4.9% mention money at all). Dataset and method:
> [The State of MCP](https://oracle.saltcrate.com/state-of-mcp) ·
> [the 595 priced servers](https://oracle.saltcrate.com/mcp-pricing).

- **Endpoint (Streamable HTTP):** `https://oracle.saltcrate.com/mcp`
- **Free tier:** **100 calls per IP, no `Authorization` header at all** — on `fetch`, `search`,
  `check_link`, `check_change`, `verify`, `monitor` and `check_deps`. Point your agent at the endpoint and go.
- **Beyond the free tier:** $0.05/call in USDC (Tempo) on every tool · $1.00/call by card on
  `extract` + `monitor` (the LLM-backed tools)
- **Starter pass (plain card, no wallet/MPP client):** **$1 for 100 calls** — <https://oracle.saltcrate.com/buy?plan=starter>.
- **No wallet?** **Prepaid access pass: $5 for 1,000 calls** → <https://oracle.saltcrate.com/buy>
- **Want a page watched without running an agent?** **SaltCrate Watch — $9 / 30 days:** 25 URLs
  checked hourly on our servers, JSON webhook on change, private dashboard, no auto-renew →
  <https://oracle.saltcrate.com/watch>

**Agent install:** [`llms-install.md`](llms-install.md) has copy-paste configs for Cursor, Cline, VS Code, Claude Code and Windsurf.

## Quickstart (zero config)

No key, no account, no payment. Any MCP client:

```bash
curl -s https://oracle.saltcrate.com/mcp \
  -H "Content-Type: application/json" \
  -H "Accept: application/json, text/event-stream" \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/call",
       "params":{"name":"search","arguments":{"query":"silver spot price"}}}'
```

That returns real results. The same endpoint answers `fetch`, `check_link`, `check_change`,
`verify`, `monitor` and `check_deps` the same way, free, up to 100 calls from your IP.
When you outgrow the free tier — or want `extract` — either settle per call (USDC/card, see below)
or buy a pass.

**Try a single tool in the browser:** <https://oracle.saltcrate.com/demo?tool=fetch> ·
[`check_link`](https://oracle.saltcrate.com/demo?tool=check_link) ·
[`search`](https://oracle.saltcrate.com/demo?tool=search&q=silver+spot+price) ·
[`extract`](https://oracle.saltcrate.com/demo?tool=extract) (page → JSON)

## Prepaid access pass (60 seconds, card, no wallet)

1. Buy a pass: <https://oracle.saltcrate.com/buy> ($5 → 1,000 calls, valid 12 months)
2. Copy the `orc_…` key it gives you.
3. Send it as a bearer token on any call:

```bash
curl -s https://oracle.saltcrate.com/mcp \
  -H "Authorization: Bearer ***" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json, text/event-stream" \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/call",
       "params":{"name":"extract","arguments":{"url":"https://example.com","schema":"{}"}}}'
```

Check remaining calls: `GET https://oracle.saltcrate.com/usage?key=orc_…`

## Pay-per-call (no account)

A call past the free allowance — or any `extract` call with no key — returns a `-32042`
"Payment Required" challenge carrying both rails: USDC via Tempo and, on `extract`/`monitor`,
a Stripe card charge. The agent pays, retries, and gets the result plus a receipt.

## Tools

| Tool | What it does | Free tier | USDC | Card |
|---|---|---|---|---|
| `fetch` | Clean extraction — boilerplate, nav, ads stripped | ✅ | $0.05 | — |
| `search` | Live web search — title, URL, snippet (DuckDuckGo / Brave / Serper) | ✅ | $0.05 | — |
| `extract` | Schema-driven structured extraction into typed JSON (DeepSeek) | — | $0.05 | $1.00 |
| `check_link` | URL liveness — status, redirect chain, final URL, latency | ✅ | $0.05 | — |
| `check_change` | Hash-based page-change detection | ✅ | $0.05 | — |
| `monitor` | Recurring URL watch — sticky change tracking with timestamps | ✅ | $0.05 | $1.00 |
| `verify` | Heuristic claim-vs-source check — confidence + supporting excerpt | ✅ | $0.05 | — |
| `check_deps` | Dependency CVE scan — latest version + OSV.dev advisories with fixing versions | ✅ | $0.05 | — |

## Watch a page on a schedule (no agent required)

Per-call monitoring is the wrong shape for a human who just wants to be told when a page moves:
50 URLs checked hourly would be ~$1,800/month at $0.05/call. So it is sold as a schedule instead —
**$9 for 30 days, 25 URLs, hourly checks, webhook alerts, private dashboard, no auto-renew:**
<https://oracle.saltcrate.com/watch>

## Use it from CI — no code required

[`page-change-monitor`](https://github.com/saltcrate/page-change-monitor) is a GitHub Action
wrapping `check_change`: watch any URL in CI, get a job-summary table plus an optional issue
listing what changed. Free tier, no API key, no state committed to your repo.

```yaml
- uses: saltcrate/page-change-monitor@v1
  with:
    urls: https://example.com/pricing
    create-issue: "true"
```

Discovery files: [`/llms.txt`](https://oracle.saltcrate.com/llms.txt) ·
[`/openapi.json`](https://oracle.saltcrate.com/openapi.json) ·
[`/pricing.md`](https://oracle.saltcrate.com/pricing.md)

Self-host (free variant): `python server.py`. Paid server: `server_paid.py` (see its module
docstring for env vars). Deployable anywhere Docker runs (`Dockerfile` + `fly.toml` included).

A [SaltCrate](https://saltcrate.com) product.
