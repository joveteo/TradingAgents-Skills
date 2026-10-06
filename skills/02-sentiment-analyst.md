# 02 · Sentiment (Social Media) Analyst

Source: `tradingagents/agents/analysts/sentiment_analyst.py`, `agents/schemas.py` (SentimentReport). The repo pre-fetches its data with no tool calls: Yahoo Finance news, StockTwits cashtag stream (30 msgs), Reddit r/wallstreetbets, r/stocks, r/investing (crypto pairs use crypto subs instead).

## Purpose
Turn the past 7 days of news framing, retail social posts, and Reddit discussion into one sentiment read. It is a signal for the Trader to weigh, not a price call.

## Inputs
Ticker, analysis date. Window = date − 7 days to date.

## Web research tasks
1. "<TICKER> news headlines <START> to <DATE>" (Yahoo Finance or major outlets). This is how institutions are framing the stock.
2. "StockTwits $<TICKER> sentiment bullish bearish <DATE>". Note the message count and the Bullish/Bearish split if shown.
3. "reddit r/wallstreetbets <TICKER> past week", plus r/stocks and r/investing. Judge posts by their body text, not titles. Do not infer engagement you can't see.
Keep only items inside the window. Leave out anything dated after the analysis date.

## How to analyze (repo's 8 rules)
1. StockTwits Bull/Bear ratio is a leading retail signal. 70/30 is moderately bullish, ≥90/10 is over-extended (contrarian risk), 50/50 is uncertain. Sample size matters.
2. A gap between sources is itself a signal, for example bearish news with bullish retail.
3. Read Reddit for substance. Subreddit character matters: WSB is exuberant or contrarian, r/stocks more measured, r/investing longer-term.
4. Separate events (news) from opinions (posts), and weight them differently.
5. Find the narrative themes that recur across sources.
6. Be honest about thin or missing data. Lower confidence and say why.
7. Pull out catalysts and risks: earnings, launches, competitors, macro.
8. Past sentiment does not predict price.

## Output format (fill every field)
- **overall_band**: exactly one of Bullish / Mildly Bullish / Neutral / Mixed / Mildly Bearish / Bearish. Use Mixed when the sources disagree. Use Neutral only when every source is quiet.
- **overall_score**: 0 (most bearish) to 10 (most bullish), 5 neutral, consistent with the band.
- **confidence**: low / medium / high, based on data quality and sample size.
- **narrative**: source-by-source breakdown, divergences, dominant themes, catalysts and risks, and a Markdown table (Signal | Direction | Source | Evidence).
