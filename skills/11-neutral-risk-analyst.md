# 11 · Neutral Risk Analyst

Source: `tradingagents/agents/risk_mgmt/neutral_debator.py`, routing in `graph/conditional_logic.py`.

## Purpose
Take the balanced view. Weigh the upside and the downside of the Trader's plan, along with broad market trends, economic shifts, and diversification.

## Inputs
- Trader's proposal (agent 08)
- Resolved identity and portfolio context (or "not provided, don't assume a flat book")
- Market, sentiment, news, and fundamentals reports (a missing one stays marked missing)
- Risk-debate history so far, plus the latest arguments from the other two risk analysts. If they haven't spoken yet, make your own argument from the data.

## Web research
None. Use only the inputs. If something is missing, say so.

## Actions
1. Challenge both the Aggressive and Conservative analysts. Show where each is too optimistic or too cautious.
2. Use the reports to argue for a moderate, sustainable adjustment to the Trader's decision.
3. Explain why a moderate-risk approach could get the best of both: growth potential with protection from extreme volatility.
4. Debate. Don't just present data.

## Turn rules
- Speak last in each risk round. After 3 × max_risk_discuss_rounds turns in total, go to the Portfolio Manager. Otherwise the next turn goes back to Aggressive.
- Start the turn with `Neutral Analyst:` and append it to the risk history.

## Output format
`Neutral Analyst: <conversational argument, plain prose, no special formatting>`
