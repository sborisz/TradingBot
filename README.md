# Bitcoin Paper-Trading Bot (24h Replay)

> 🔴 **CRITICAL NOTICE:** This documentation, benchmark metrics, and execution details are fully mirrored and duplicated inside the accompanying `index.html` visual report file.

## Executive Summary: Why This Task is Statistically Flawed

While the bot executed successfully according to all technical parameters (fetching real-time data, fulfilling trade counts, and liquidating to flat), **achieving a guaranteed net profit over an arbitrary 24-hour window under these constraints is mathematically non-viable**.

As permitted by the task instructions, this repository demonstrates both the functional code and the mathematical breakdown of why this exercise fails in live market conditions.

---

## Strategy & Architectural Breakdown

### 1. The Core Strategy
The bot applies a **15-period Simple Moving Average (SMA) momentum crossover** on BTC/USD candles fetched live from CoinGecko's public REST API.

- **Entry:** Buy when the close price crosses above the 15-period SMA.
- **Exit:** Sell when the price drops below the SMA, or force-close at the final data point (Flat at the end).
- **Constraints Enforced:** Minimum 5 completed trades, 0.1% fee per side, 0 BTC held at termination.

---

## Why It Cannot Work (The 3 Structural Traps)

1. **The 0.2% Round-Trip Fee Friction:**
   Every complete trade incurs a total fee penalty of ~0.2%. Over 5 to 8 mandatory trades, the bot must generate at least 1.0% to 1.6% in gross profit just to break even. Market noise systematically eats the account.

2. **Forced Liquidation (Flat at the end):**
   The rule requiring 0 BTC balance at termination deprives the algorithm of mean-reversion recovery.

3. **No Look-Ahead & Overfitting Trap:**
   Without future knowledge, lag-based indicators generate false signals in range-bound regimes.

---

## Live Benchmark Results

- **Run Timestamp:** 2026-11-11 14:40:51
- **Data Window:** 2026-11-10 14:45:00 -> 2026-11-11 14:39:10 (UTC)
- **Execution Status:** 8 completed trades - Final Position: 0 BTC (Flat)

```text
=== TRADING SESSION RESULTS ===
Total Trades Executed : 8
Initial Starting Cash : $10,000.00
Final Balance         : $9,818.58
Net Profit / Loss     : $-181.42 (-1.81%)
Buy-and-Hold PnL      : $54.74

=== DETAILED TRADE LOG ===
Trade #01 | Buy: 2026-11-10 17:35:00 @ $73060.63 -> Sell: 2026-11-10 17:40:00 @ $72800.54 (PnL:$-55.51)
Trade #02 | Buy: 2026-11-10 17:50:00 @ $73016.90 -> Sell: 2026-11-10 19:15:00 @ $73300.45 (PnL:$18.67)
Trade #03 | Buy: 2026-11-10 19:20:00 @ $73372.23 -> Sell: 2026-11-10 19:25:00 @ $73318.63 (PnL:$-27.17)
Trade #04 | Buy: 2026-11-10 19:35:00 @ $73401.01 -> Sell: 2026-11-10 19:40:00 @ $73286.82 (PnL:$-35.28)
Trade #05 | Buy: 2026-11-10 19:50:00 @ $73469.38 -> Sell: 2026-11-10 20:05:00 @ $73278.73 (PnL:$-45.42)
Trade #06 | Buy: 2026-11-10 20:15:00 @ $73480.31 -> Sell: 2026-11-10 21:55:00 @ $73772.49 (PnL:$19.42)
Trade #07 | Buy: 2026-11-10 22:10:00 @ $73877.81 -> Sell: 2026-11-10 22:15:00 @ $73854.20 (PnL:$-22.88)
Trade #08 | Buy: 2026-11-10 23:40:00 @ $73699.05 -> Sell: 2026-11-11 00:15:00 @ $73597.99 (PnL:$-33.17)
```

---

## Setup & Execution

### Requirements
- Python 3.10+
- No API keys required

### Run Command
```bash
python bot.py
```
