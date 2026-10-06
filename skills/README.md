# TradingAgents skills

Plain-language, per-agent instruction files derived from this repo's agents at commit `1394a3f`. Upload them to a chat-tool project (for example a Perplexity Project) so the TradingAgents workflow can run without Python or API keys.

## Mapping to the repo

- Agent prompt files under `tradingagents/agents/` → `01-market-analyst.md` through `12-portfolio-manager.md`
- Graph wiring and defaults (`tradingagents/graph/setup.py`, `graph/conditional_logic.py`, `default_config.py`) → `00-orchestrator.md`
- `space-instructions.txt` is the project-level instruction file for the chat tool

## How to use

1. Create a project in a chat tool.
2. Paste `space-instructions.txt` as the project instructions.
3. Upload the 13 `.md` files (`00-orchestrator.md` through `12-portfolio-manager.md`).
4. Ask, for example: `Run TradingAgents on NVDA`.

You can also give an analysis date and any existing position so the Trader, risk debate, and Portfolio Manager can factor them in.

## Known gap

The repo's decision memory log and reflection vs SPY (`tradingagents/memory/`) is not ported. A later Python run settles past decisions against 5-day returns vs SPY and passes lessons to the Portfolio Manager. A chat-tool project has no such log. If you paste an earlier run's outcome, agent 12 can treat it as lessons; otherwise it is skipped.

## Example

A full sample run on AAPL (hypothetical demo position, analysis date 6 Oct 2026) is in [`examples/AAPL-sample-2026-10-06.md`](examples/AAPL-sample-2026-10-06.md).

Research only; not financial advice.
