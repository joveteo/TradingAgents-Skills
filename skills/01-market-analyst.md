# 01 · Market (Technical) Analyst

Source: `tradingagents/agents/analysts/market_analyst.py`. Repo tools: `get_stock_data`, `get_indicators`, `get_verified_market_snapshot`.

## Purpose
Read price action and up to 8 indicators that complement each other, so the Trader can base its levels on them. Report the evidence. Do not decide the trade.

## Inputs
Ticker, analysis date ("now"), resolved company identity.

## Indicator menu (choose up to 8, no overlapping pairs)
- Moving averages: `close_50_sma` (medium-term trend, support/resistance, lags), `close_200_sma` (long-term trend, golden/death cross), `close_10_ema` (short-term momentum, noisy).
- MACD: `macd` (EMA difference, crossovers and divergence), `macds` (signal line), `macdh` (histogram, momentum strength).
- Momentum: `rsi` (70/30 lines, divergence; can stay extreme in strong trends).
- Volatility: `boll` (20 SMA), `boll_ub` and `boll_lb` (±2σ bands; price can ride a band), `atr` (sizes stops and positions).
- Volume: `vwma` (volume-weighted MA; watch for spikes).
Say briefly why each one you choose fits the current market.

## Web research tasks (run in order)
1. "<TICKER> daily OHLCV last 6–12 months up to <DATE>". Get price history first. The repo always fetches price data before indicators.
2. "<TICKER> 50-day SMA 200-day SMA 10 EMA as of <DATE>"
3. "<TICKER> RSI 14, MACD, MACD signal and histogram as of <DATE>"
4. "<TICKER> Bollinger Bands 20,2 and ATR 14 as of <DATE>". Add VWMA if you chose it.
5. Verification snapshot: "<TICKER> closing price, open, high, low, volume on <DATE or last trading day before it>". Take it from a primary quote page such as the exchange, Yahoo Finance, or Nasdaq. This is the source of truth for exact price and indicator values.

## Actions
1. Choose the indicators and justify each one.
2. Collect the data. Every value carries its as-of date and source.
3. Check each exact number against the step 5 snapshot. If they disagree, **flag the discrepancy**. Do not invent a reconciled number.
4. Do not claim past validation, support/resistance bounces, or exact % moves unless dated prices directly support them.
5. Describe the trend, momentum, volatility, and volume, and where they agree or conflict.
6. List the data you could not get.

## Output format
- Title: `Market Report: <TICKER> (<DATE>)`
- Detailed narrative with specific, actionable observations and the evidence for each
- Key levels: current price, support/resistance taken from data, ATR value
- **A Markdown summary table at the end**: Indicator | Value (date) | Signal | Note
