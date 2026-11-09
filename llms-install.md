# Install SaltCrate Oracle (MCP server)

SaltCrate Oracle is a **remote MCP server** (Streamable HTTP). There is nothing to install,
build, or host. It needs no API key, no account, and no wallet:

- **Endpoint:** `https://oracle.saltcrate.com/mcp`
- **Free tier:** 200 calls/hour per IP on `fetch`, `search`, `check_link`, `check_change`,
  `verify`, `monitor` — no `Authorization` header at all.
- **Paid:** $0.01/call in USDC (Tempo) on all 7 tools; $0.50/call by card on `extract` +
  `monitor`; or a prepaid pass — **$5 for 1,000 calls** at https://oracle.saltcrate.com/buy
- **Tools:** `search`, `fetch`, `extract`, `check_link`, `check_change`, `verify`, `monitor`
- **Docs:** https://oracle.saltcrate.com/llms.txt · OpenAPI: https://oracle.saltcrate.com/openapi.json

## Claude Code

```bash
claude mcp add --transport http saltcrate-oracle https://oracle.saltcrate.com/mcp
```

## Cursor

Add to `~/.cursor/mcp.json` (or `.cursor/mcp.json` in a project):

```json
{
  "mcpServers": {
    "saltcrate-oracle": {
      "url": "https://oracle.saltcrate.com/mcp"
    }
  }
}
```

## Cline

MCP Servers → Remote Servers → Add:

```json
{
  "mcpServers": {
    "saltcrate-oracle": {
      "type": "streamableHttp",
      "url": "https://oracle.saltcrate.com/mcp"
    }
  }
}
```

## VS Code (GitHub Copilot agent mode)

Add to `.vscode/mcp.json`:

```json
{
  "servers": {
    "saltcrate-oracle": {
      "type": "http",
      "url": "https://oracle.saltcrate.com/mcp"
    }
  }
}
```

## Windsurf

Add to `~/.codeium/windsurf/mcp_config.json`:

```json
{
  "mcpServers": {
    "saltcrate-oracle": {
      "serverUrl": "https://oracle.saltcrate.com/mcp"
    }
  }
}
```

## Claude Desktop (via mcp-remote bridge)

Claude Desktop speaks stdio only; bridge it:

```json
{
  "mcpServers": {
    "saltcrate-oracle": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://oracle.saltcrate.com/mcp"]
    }
  }
}
```

## Any HTTP client

```bash
curl -s https://oracle.saltcrate.com/mcp \
  -H "Content-Type: application/json" \
  -H "Accept: application/json, text/event-stream" \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/call",
       "params":{"name":"search","arguments":{"query":"silver spot price"}}}'
```

With a prepaid pass, add `-H "Authorization: Bearer orc_…"` and `extract` works too.

## Verify it works

```bash
curl -s https://oracle.saltcrate.com/mcp \
  -H "Content-Type: application/json" \
  -H "Accept: application/json, text/event-stream" \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/list"}'
```
