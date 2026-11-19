# Adanos plugin

This plugin connects your agent to [Adanos](https://adanos.com). Adanos measures how often stocks and ETFs are discussed on Reddit, X, News and Polymarket. It also measures how positive or negative that talk is.

With the plugin your agent can answer questions like these:

- Which stocks are most discussed on Reddit today?
- How is NVDA discussed on Reddit, X and News this week?
- Why is TSLA discussed right now?
- How does the buzz of AAPL, MSFT and GOOGL compare on X?
- How does the stock market feel today?

The plugin adds two parts:

- The Adanos MCP server at `https://adanos.com/mcp`. Its tools only read data.
- The `adanos` skill. It tells the agent which tool answers which question, what each call costs and which period each tool uses by default.

## Before you start

You need an Adanos account. A free account works. Your agent signs you in through a browser window. The plugin holds no API key.

Each tool call costs API calls from the monthly quota of your plan. `get_mentions` and `analyze_text` are on the Professional plan only. On the other plans the server leaves the two tools out of the tool list. The plans are at [adanos.com/pricing](https://adanos.com/pricing).

## Install

### Claude Code

```bash
claude plugin marketplace add adanos-software/adanos-mcp
claude plugin install adanos@adanos
```

Start a new session. Run `/mcp`, select `plugin:adanos:adanos` and sign in.

### Codex

```bash
codex plugin marketplace add adanos-software/adanos-mcp
codex plugin add adanos@adanos
codex mcp login adanos
```

### Cursor

```bash
git clone https://github.com/adanos-software/adanos-mcp.git ~/.cursor/plugins/local/adanos
```

Run **Developer: Reload Window**. Open **Customize** to check that the `adanos` skill and MCP server are there.

### Grok Build

```bash
grok plugin install adanos-software/adanos-mcp --trust
```

Start a new session. Open `/mcps`, select `adanos` and press `i` to sign in.

### Kiro

[Add to Kiro](https://kiro.dev/launch/mcp/add?name=adanos&config=%7B%22url%22%3A%22https%3A//adanos.com/mcp%22%2C%22disabled%22%3Afalse%2C%22autoApprove%22%3A%5B%5D%7D)

Kiro signs you in with OAuth in the browser.

### Other clients

Add a remote MCP server with this URL:

```text
https://adanos.com/mcp
```

The transport is Streamable HTTP. Sign-in uses OAuth. The client finds the OAuth endpoints through the server. A client without OAuth can send an Adanos API key in the `Authorization: Bearer` header or the `x-api-key` header.

## Tools

| Tool | What it answers | API calls |
| --- | --- | --- |
| `get_trending` | The most discussed stocks, ETFs, sectors or countries on one source | 1 |
| `get_ticker` | Buzz and sentiment of one stock per source, with short snippets | 1 per source |
| `explain_ticker` | Why a stock is discussed, per source | 1 per source |
| `compare_tickers` | Up to 10 stocks side by side on one source | 1 |
| `get_market_sentiment` | The mood of the whole stock market per source | 1 per source |
| `search` | Find a stock or ETF by name or ticker | 1 |
| `get_mentions` | The latest posts about one stock on one source. Professional plan only | 1 |
| `analyze_text` | The sentiment of a finance text. Professional plan only | 1 |
| `get_usage` | Your plan and the calls you have left | 0 when signed in, 1 with an API key |

## Data and privacy

The plugin sends your agent's tool calls to `https://adanos.com/mcp`. Nothing else leaves your machine through it. The plugin runs no local code and no hooks.

- Privacy policy: [adanos.com/privacy](https://adanos.com/privacy)
- Terms: [adanos.com/terms](https://adanos.com/terms)
- Support: [adanos.com/support](https://adanos.com/support) or support@adanos.com

## Files

| File | Read by |
| --- | --- |
| `mcp.json` | Codex, Cursor (Agent Plugins format) |
| `.mcp.json` | Claude Code, Grok Build. Same content as `mcp.json` |
| `plugin.json` | Codex, Cursor (Agent Plugins format) |
| `.claude-plugin/plugin.json` | Claude Code, Grok Build |
| `.claude-plugin/marketplace.json` | Claude Code |
| `.agents/plugins/marketplace.json` | Codex |
| `skills/adanos/SKILL.md` | All clients |
| `server.json` | The [official MCP Registry](https://registry.modelcontextprotocol.io) as `com.adanos/mcp` |
| `assets/icon.png` | The icon for directory listings |

## License

MIT. See [LICENSE](LICENSE).
