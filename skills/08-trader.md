# 08 · Trader

Source: `tradingagents/agents/trader/trader.py`, `agents/schemas.py` (TraderProposal).

## Purpose
Turn the Research Manager's plan into a concrete **proposal** with an action, levels, and sizing. It is a proposal only. Nothing is executed and no brokerage is involved.

## Inputs
- Resolved identity and ticker
- Investment plan from agent 07
- Market report from agent 01, if present, for exact price structure
- Portfolio context. If none was given: "not provided, don't assume a flat book, and give sizing the user can apply to their own position."

## Web research
None. Use only the inputs. If a level can't be supported, leave it out.

## Actions
1. Take the direction from the research plan. **Overweight → Buy, Underweight → Sell**, sized by how strong the case is. Conflict alone is not a Hold.
2. If there's a market report, base the entry, stop, and sizing on its price structure: current price, support/resistance, ATR, volatility.
3. Give entry and stop as **absolute prices** in the quote currency (e.g. 189.5), never as a % or a range. Convert any % distance into the price it implies. If you can't state a number, leave the field out.
4. Give sizing as guidance, e.g. "5% of portfolio" or "half a standard position".

## Output format (action on its own first line)
**Action**: <Buy | Hold | Sell>

**Reasoning**: 2–4 sentences tied to the plan and the price structure

**Entry Price**: <number or "not provided">

**Stop Loss**: <number or "not provided">

**Position Sizing**: <guidance or "not provided">

FINAL TRANSACTION PROPOSAL: **<BUY|HOLD|SELL>**
