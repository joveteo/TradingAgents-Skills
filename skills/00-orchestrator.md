# 00 · TradingAgents Orchestrator (run order)

Source: TauricResearch/TradingAgents `tradingagents/graph/setup.py`, `graph/conditional_logic.py`, `default_config.py` (commit 1394a3f, 3 Oct 2026).

## Purpose
Run a multi-agent equity research pass on one ticker and end with one rating and a BUY/HOLD/SELL call. Research only: never place, simulate, or suggest connecting to a trade or brokerage.

## Inputs
- **Ticker** (required). Keep the exact symbol and any exchange suffix (`.TO`, `.L`, `.HK`, `-USD`).
- **Analysis date** (optional, default today, YYYY-MM-DD). Treat it as "now". Use no data published after it.
- **Portfolio context** (optional). If none is given, say so. Do not assume the user holds nothing.

## Step 0: Pin the identity
Look up the company's name, sector, industry, and exchange for the ticker. Every agent uses that identity. Never swap in a different company with a similar chart or name. A crypto pair is an asset, not a company.

## Run order (each step is one agent file)
| Step | Agent file | Reads | Writes |
|---|---|---|---|
| 1 | 01 Market analyst | ticker, date | market_report |
| 1 | 02 Sentiment analyst | ticker, date | sentiment_report |
| 1 | 03 News analyst | ticker, date | news_report |
| 1 | 04 Fundamentals analyst | ticker, date | fundamentals_report |
| 2 | 05 Bull ⇄ 06 Bear | 4 reports + debate so far | investment debate |
| 3 | 07 Research manager | debate history | investment_plan (5-tier) |
| 4 | 08 Trader | plan + market_report + portfolio | trader_plan (Buy/Hold/Sell) |
| 5 | 09 Aggressive → 10 Conservative → 11 Neutral | trader_plan + 4 reports | risk debate |
| 6 | 12 Portfolio manager | risk debate + plan + trader_plan | final rating |

The four step-1 analysts run independently, in any order. Steps 2 onward run only after all four reports exist. A missing report is written as "(No X report in this run: not available, not an empty finding)". Later agents must not invent its contents.

## Debate rounds (repo defaults)
- Investment debate: `max_debate_rounds = 1`. Bull speaks first, then Bear, so there are 2 turns. With N rounds, alternate until 2·N turns.
- Risk debate: `max_risk_discuss_rounds = 1`. Aggressive, then Conservative, then Neutral, so there are 3 turns. With N rounds, repeat until 3·N turns.
- Prefix each turn with its speaker ("Bull Analyst:", "Aggressive Analyst:" …) and append it to a running history.
- The first speaker in a debate has no opponent yet and argues from the data. It must not rebut points nobody made.

## Evidence rules (all agents)
- Only the four analysts research the web. Agents 05–12 use only the reports and debate text in front of them. If something is missing, they say so.
- Give every number a date and a source. If sources conflict, flag the conflict. Do not average the numbers into a new one.
- An analyst reports evidence. Analysts do not decide the trade.

## Final output
1. A one-line header: ticker, company, analysis date.
2. Each agent's output under its own heading, collapsed or summarized if long.
3. **Final Rating**, the Portfolio Manager's 5-tier rating: Buy / Overweight / Hold / Underweight / Sell.
4. **FINAL DECISION: BUY / HOLD / SELL**, mapped this way: Buy or Overweight → BUY. Hold → HOLD. Underweight or Sell → SELL. This is the same mapping the repo's Trader uses.
5. If no rating can be read, output **REVIEW** and do not default to HOLD. The repo uses REVIEW the same way.
6. One line: "Research output, not financial advice."

## Not ported (repo-only)
The repo keeps a memory log of past decisions. A later run settles each one against 5-day returns vs SPY, writes a 2–4 sentence reflection, and passes it to the Portfolio Manager as "lessons". A Space has no such log. If the user pastes an earlier run's outcome, hand it to agent 12 as lessons. Otherwise skip it.
