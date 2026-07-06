<h1 id="for-ai-agents">For AI Agents</h1>

Anboto ships first-party tooling that connects AI agents (Claude, Cursor,
Codex, or any MCP-compatible client) to the Trading API — including the
execution algos (TWAP, VWAP, POV, IS, ICEBERG) that make Anboto an execution
desk rather than an order button.

**Everything lives in the open-source
[anboto-agent-kit](https://github.com/anbotolabs/anboto-agent-kit)** (MIT):

- `@anboto/mcp` — MCP server (stdio) with 22 tools: order creation and
  monitoring, portfolio, market data, preflight validation and guardrails
- `@anboto/core` — typed TypeScript client (HMAC/RSA signing included)
- `anboto-trading` — a Claude Skill teaching agents algo selection,
  execution monitoring and TCA

## Quickstart (Claude Code)

```shell
claude mcp add anboto \
  -e ANBOTO_API_KEY=your-key \
  -e ANBOTO_API_SECRET=your-base64-secret \
  -- npx -y @anboto/mcp --testnet
```

Claude Desktop / Cursor (JSON config):

```json
{
  "mcpServers": {
    "anboto": {
      "command": "npx",
      "args": ["-y", "@anboto/mcp", "--testnet"],
      "env": {
        "ANBOTO_API_KEY": "your-key",
        "ANBOTO_API_SECRET": "your-base64-secret"
      }
    }
  }
}
```

Then ask the agent: *"On testnet, buy 0.001 BTC on Binance via TWAP over 5
minutes, monitor to completion, and give me an execution report."*

## Guardrails

API keys stay on your machine (env vars only — never in the model context).
Optional flags, all fail-closed:

| Flag | Effect |
|------|--------|
| `--testnet` | Simulated environment (recommended to start) |
| `--read-only` | Trading tools are not registered at all |
| `--max-order-notional=50000` | Reject orders above an estimated notional |
| `--allowed-exchanges=BINANCE,OKX` | Exchange allowlist |
| `--allowed-symbols=BTC/USDT` | Symbol allowlist |

## Machine-readable docs

- llms.txt index: [/llms.txt](llms.txt) · full docs as one file: [/llms-full.txt](llms-full.txt)
- OpenAPI (REST): [anboto-trading-api-2.0.yml](anboto-trading-api-2.0.yml)
- AsyncAPI (WebSocket): [asynapi.yml](asynapi.yml)
- Raw Markdown of this site: [index.md](index.md), [websocket.md](websocket.md)
