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

- **Run Timestamp:** 2026-11-26 19:29:22
- **Data Window:** 2026-11-25 19:30:00 -> 2026-11-26 19:28:00 (UTC)
- **Execution Status:** 8 completed trades - Final Position: 0 BTC (Flat)

```text
=== TRADING SESSION RESULTS ===
Total Trades Executed : 8
Initial Starting Cash : $10,000.00
Final Balance         : $9,883.94
Net Profit / Loss     : $-116.06 (-1.16%)
Buy-and-Hold PnL      : $61.42

=== DETAILED TRADE LOG ===
Trade #01 | Buy: 2026-11-25 21:10:00 @ $60177.65 -> Sell: 2026-11-25 22:00:00 @ $60189.22 (PnL:$-18.06)
Trade #02 | Buy: 2026-11-25 22:50:00 @ $60137.81 -> Sell: 2026-11-25 22:55:00 @ $60080.11 (PnL:$-29.50)
Trade #03 | Buy: 2026-11-25 23:15:00 @ $60150.09 -> Sell: 2026-11-26 00:50:00 @ $60438.62 (PnL:$27.76)
Trade #04 | Buy: 2026-11-26 01:05:00 @ $60606.38 -> Sell: 2026-11-26 01:10:00 @ $60581.41 (PnL:$-24.05)
Trade #05 | Buy: 2026-11-26 01:15:00 @ $60705.19 -> Sell: 2026-11-26 02:55:00 @ $60689.76 (PnL:$-22.42)
Trade #06 | Buy: 2026-11-26 03:50:00 @ $60782.41 -> Sell: 2026-11-26 05:05:00 @ $60888.11 (PnL:$-2.61)
Trade #07 | Buy: 2026-11-26 05:25:00 @ $60921.32 -> Sell: 2026-11-26 05:40:00 @ $60912.86 (PnL:$-21.22)
Trade #08 | Buy: 2026-11-26 07:30:00 @ $60505.46 -> Sell: 2026-11-26 08:00:00 @ $60468.22 (PnL:$-25.89)
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
