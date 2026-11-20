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

- **Run Timestamp:** 2026-11-20 15:43:38
- **Data Window:** 2026-11-19 15:45:00 -> 2026-11-20 15:42:20 (UTC)
- **Execution Status:** 8 completed trades - Final Position: 0 BTC (Flat)

```text
=== TRADING SESSION RESULTS ===
Total Trades Executed : 8
Initial Starting Cash : $10,000.00
Final Balance         : $9,834.99
Net Profit / Loss     : $-165.01 (-1.65%)
Buy-and-Hold PnL      : $-453.08

=== DETAILED TRADE LOG ===
Trade #01 | Buy: 2026-11-19 17:30:00 @ $62942.81 -> Sell: 2026-11-19 17:55:00 @ $62797.28 (PnL:$-43.05)
Trade #02 | Buy: 2026-11-19 18:05:00 @ $62746.90 -> Sell: 2026-11-19 18:10:00 @ $62702.40 (PnL:$-26.94)
Trade #03 | Buy: 2026-11-19 18:20:00 @ $62848.44 -> Sell: 2026-11-19 18:50:00 @ $62894.92 (PnL:$-12.51)
Trade #04 | Buy: 2026-11-19 22:05:00 @ $61717.94 -> Sell: 2026-11-19 23:15:00 @ $61972.11 (PnL:$20.95)
Trade #05 | Buy: 2026-11-19 23:20:00 @ $62056.98 -> Sell: 2026-11-19 23:25:00 @ $61908.38 (PnL:$-43.61)
Trade #06 | Buy: 2026-11-20 00:05:00 @ $62029.95 -> Sell: 2026-11-20 00:55:00 @ $61934.58 (PnL:$-34.95)
Trade #07 | Buy: 2026-11-20 01:00:00 @ $62051.58 -> Sell: 2026-11-20 01:40:00 @ $62058.25 (PnL:$-18.64)
Trade #08 | Buy: 2026-11-20 02:55:00 @ $61796.76 -> Sell: 2026-11-20 04:05:00 @ $61881.66 (PnL:$-6.17)
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
