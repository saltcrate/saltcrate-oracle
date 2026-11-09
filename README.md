# Oracle by SaltCrate

**Live web intelligence for AI agents — free to start, no key, no signup, no wallet.** Seven MCP
tools: live search, schema-driven structured extraction, clean page fetch, link checking, change
detection, recurring monitoring, and claim verification.

- **Endpoint (Streamable HTTP):** `https://oracle.saltcrate.com/mcp`
- **Free tier:** **200 calls/hour per IP, no `Authorization` header at all** — on `fetch`, `search`,
  `check_link`, `check_change`, `verify` and `monitor`. Point your agent at the endpoint and go.
- **Beyond the free tier:** $0.01/call in USDC (Tempo) on every tool · $0.50/call by card on
  `extract` + `monitor` (the LLM-backed tools)
- **No wallet?** **Prepaid access pass: $5 for 1,000 calls** → <https://oracle.saltcrate.com/buy>

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
`verify` and `monitor` the same way, free, up to 200 calls/hour from your IP. When you outgrow
the free tier — or want `extract` — either settle per call (USDC/card, see below) or buy a pass.

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
  -H "Authorization: Bearer orc_…" \
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
| `fetch` | Clean extraction — boilerplate, nav, ads stripped | ✅ | $0.01 | — |
| `search` | Live web search — title, URL, snippet (DuckDuckGo / Brave / Serper) | ✅ | $0.01 | — |
| `extract` | Schema-driven structured extraction into typed JSON (DeepSeek) | — | $0.01 | $0.50 |
| `check_link` | URL liveness — status, redirect chain, final URL, latency | ✅ | $0.01 | — |
| `check_change` | Hash-based page-change detection | ✅ | $0.01 | — |
| `monitor` | Recurring URL watch — sticky change tracking with timestamps | ✅ | $0.01 | $0.50 |
| `verify` | Heuristic claim-vs-source check — confidence + supporting excerpt | ✅ | $0.01 | — |

Discovery files: [`/llms.txt`](https://oracle.saltcrate.com/llms.txt) ·
[`/openapi.json`](https://oracle.saltcrate.com/openapi.json) ·
[`/pricing.md`](https://oracle.saltcrate.com/pricing.md)

Self-host (free variant): `python server.py`. Paid server: `server_paid.py` (see its module
docstring for env vars). Deployable anywhere Docker runs (`Dockerfile` + `fly.toml` included).

A [SaltCrate](https://saltcrate.com) product.
