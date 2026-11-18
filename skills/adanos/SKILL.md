---
name: adanos
description: Answer questions about how stocks and ETFs are discussed on Reddit, X, News and Polymarket with the Adanos MCP tools. Use when the user asks which stocks are trending or discussed, about the buzz or sentiment of a ticker, why a stock is discussed, to compare tickers or about the mood of the stock market. Picks the right tool and keeps the API-call cost low.
---

# Adanos

The Adanos MCP server answers how often stocks and ETFs are discussed on Reddit, X, News and Polymarket and how positive or negative that talk is. Every tool only reads data.

Each tool call costs API calls from the user's monthly quota. Pick the tool that answers the question in the fewest calls.

## Pick the tool

| Question | Tool | API calls |
| --- | --- | --- |
| What is discussed now? | `get_trending` | 1 |
| How does one ticker look? | `get_ticker` | 1 per source, up to 4 |
| Why is a stock discussed? | `explain_ticker` | 1 per source, 3 by default |
| How do several tickers compare? | `compare_tickers` | 1 |
| How does the whole market feel? | `get_market_sentiment` | 1 per source, up to 4 |
| Which ticker has this name? | `search` | 1 |
| How positive or negative is this text? | `analyze_text` | 1 |
| What were the latest posts about a ticker? | `get_mentions` | 1 |
| Which plan, how many calls are left? | `get_usage` | 0 when signed in, 1 with an API key |

`get_mentions` and `analyze_text` are on the Professional plan only. When an account on another plan is signed in, the two tools are not in the tool list. Answer with the other tools.

## Keep the cost low

- `get_ticker` and `get_market_sentiment` call every source by default. Pass `sources` when the user names one or two, for example `["reddit"]`.
- `explain_ticker` calls Reddit, X and News by default. Polymarket has no explanations.
- `get_trending`, `compare_tickers` and `get_mentions` read one source. Pass `source`: `reddit`, `x`, `news` or `polymarket`.
- To compare up to 10 tickers, call `compare_tickers` once. Do not call `get_ticker` for each ticker.
- Call `search` only when the user gives a company name and you do not know the ticker.
- `include_history` adds daily rows to the same call. It costs no extra call, but it makes the answer longer. Pass it only when the user asks for a trend over days.

## Periods

- `from` and `to` take days as `YYYY-MM-DD`, in UTC, both inclusive.
- `get_trending` and `get_market_sentiment` default to today.
- `get_ticker`, `get_mentions` and `compare_tickers` default to the last 7 days.
- `explain_ticker` takes no period.
- How far back a period can reach depends on the plan. `get_usage` lists the history days of each plan.

## Lists

- `get_trending` takes `kind`: `stocks` (default), `etfs`, `all`, `sectors` or `countries`. `limit` is 10 by default and at most 50.
- `search` returns 5 matches by default and at most 20.
- `get_mentions` returns 10 rows by default and at most 50.

## Read the answers

- Answers are compact text and CSV. Quote the numbers as the tool gives them.
- When a source has no data for the period, the answer says so for that source. Report it and do not guess a value.
- When less than 10% of the month's calls are left, the answer ends with a line such as `Quota: 20 of 250 calls left, resets 2026-12-01.` Tell the user.
- When a tool answers with a limit, such as a quota that is used up or a period longer than the plan allows, pass the sentence on to the user as it is.
