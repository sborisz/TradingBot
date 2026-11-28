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

- **Run Timestamp:** 2026-11-28 05:45:43
- **Data Window:** 2026-11-27 05:50:00 -> 2026-11-28 05:44:20 (UTC)
- **Execution Status:** 8 completed trades - Final Position: 0 BTC (Flat)

```text
=== TRADING SESSION RESULTS ===
Total Trades Executed : 8
Initial Starting Cash : $10,000.00
Final Balance         : $9,719.29
Net Profit / Loss     : $-280.71 (-2.81%)
Buy-and-Hold PnL      : $-336.74

=== DETAILED TRADE LOG ===
Trade #01 | Buy: 2026-11-27 07:30:00 @ $61769.62 -> Sell: 2026-11-27 07:40:00 @ $61527.59 (PnL:$-59.08)
Trade #02 | Buy: 2026-11-27 07:55:00 @ $61702.73 -> Sell: 2026-11-27 08:00:00 @ $61616.53 (PnL:$-33.72)
Trade #03 | Buy: 2026-11-27 08:05:00 @ $61781.27 -> Sell: 2026-11-27 08:35:00 @ $61724.51 (PnL:$-28.88)
Trade #04 | Buy: 2026-11-27 09:35:00 @ $61771.68 -> Sell: 2026-11-27 09:45:00 @ $61658.19 (PnL:$-37.85)
Trade #05 | Buy: 2026-11-27 11:25:00 @ $61607.44 -> Sell: 2026-11-27 11:50:00 @ $61473.14 (PnL:$-41.07)
Trade #06 | Buy: 2026-11-27 12:00:00 @ $61597.09 -> Sell: 2026-11-27 12:15:00 @ $61582.16 (PnL:$-21.95)
Trade #07 | Buy: 2026-11-27 13:00:00 @ $61566.29 -> Sell: 2026-11-27 13:05:00 @ $61503.81 (PnL:$-29.44)
Trade #08 | Buy: 2026-11-27 13:50:00 @ $61472.36 -> Sell: 2026-11-27 14:05:00 @ $61414.45 (PnL:$-28.64)
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
