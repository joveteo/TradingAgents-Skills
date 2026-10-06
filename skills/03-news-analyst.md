# 03 · News & Macro Analyst

Source: `tradingagents/agents/analysts/news_analyst.py`, `agents/tools.py`. Repo tools: `get_news`, `get_global_news`, `get_macro_indicators` (FRED), `get_prediction_markets` (Polymarket).

## Purpose
Report on the past week of the world as it bears on this company and on trading and macro conditions, with the macro claims backed by actual data.

## Inputs
Ticker, analysis date, resolved company identity ("company", or "asset" for crypto).

## Web research tasks
1. Company news: "<COMPANY> <TICKER> news <DATE−7d> to <DATE>". Earnings, guidance, products, legal and regulatory events, management, deals.
2. Global and macro news: "global markets macroeconomic news week of <DATE>". Central banks, geopolitics, trade policy, the stock's sector.
3. Macro data. Back up any macro claim with the latest value and its trend, taken from FRED or the official source:
   - "US CPI latest", "core PCE latest", "US unemployment rate latest"
   - "fed funds rate current", "10-year Treasury yield <DATE>", "2s10s yield curve <DATE>"
   - Use real GDP or VIX when relevant. For euro-area exposure, use ECB deposit rate, HICP, Bund yield, EUR/USD.
4. Prediction markets: "Polymarket odds Fed rate cut <next meeting>", "Polymarket recession <year>", and any market tied to the sector or company (elections, tariffs, export controls). Record the implied probability, volume, and resolution date.
Use only items published on or before the analysis date.

## Actions
1. Collect the company news, then the macro news, then the data.
2. Tie each macro point to a number with its date and source.
3. Separate confirmed events from rumor and opinion.
4. Spell out how each item could move this stock, and in which direction.
5. Report only what the sources support. Another agent decides the trade. List the gaps.

## Output format
- Title: `News & Macro Report: <TICKER> (<DATE>)`
- Sections: Company-specific news · Sector/competitor news · Macro backdrop (with data) · Event probabilities · Key catalysts and risks
- Specific, actionable insights with evidence
- **A Markdown summary table at the end**: Item | Date | Source | Direction for stock | Importance
