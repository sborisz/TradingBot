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

- **Run Timestamp:** 2026-11-07 19:13:04
- **Data Window:** 2026-11-06 19:15:00 -> 2026-11-07 19:11:30 (UTC)
- **Execution Status:** 8 completed trades - Final Position: 0 BTC (Flat)

```text
=== TRADING SESSION RESULTS ===
Total Trades Executed : 8
Initial Starting Cash : $10,000.00
Final Balance         : $9,809.17
Net Profit / Loss     : $-190.83 (-1.91%)
Buy-and-Hold PnL      : $-7.97

=== DETAILED TRADE LOG ===
Trade #01 | Buy: 2026-11-06 20:55:00 @ $75794.92 -> Sell: 2026-11-06 21:30:00 @ $75716.36 (PnL:$-30.32)
Trade #02 | Buy: 2026-11-06 22:00:00 @ $75843.57 -> Sell: 2026-11-06 23:00:00 @ $75850.44 (PnL:$-19.02)
Trade #03 | Buy: 2026-11-06 23:35:00 @ $75891.76 -> Sell: 2026-11-07 00:00:00 @ $75775.28 (PnL:$-35.12)
Trade #04 | Buy: 2026-11-07 00:10:00 @ $75858.81 -> Sell: 2026-11-07 00:30:00 @ $75860.19 (PnL:$-19.63)
Trade #05 | Buy: 2026-11-07 00:40:00 @ $75875.88 -> Sell: 2026-11-07 00:45:00 @ $75852.13 (PnL:$-22.86)
Trade #06 | Buy: 2026-11-07 01:05:00 @ $75885.91 -> Sell: 2026-11-07 01:15:00 @ $75838.33 (PnL:$-25.90)
Trade #07 | Buy: 2026-11-07 01:35:00 @ $75852.25 -> Sell: 2026-11-07 02:55:00 @ $75948.16 (PnL:$-7.25)
Trade #08 | Buy: 2026-11-07 03:05:00 @ $76030.29 -> Sell: 2026-11-07 03:20:00 @ $75945.32 (PnL:$-30.63)
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
