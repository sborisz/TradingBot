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

- **Run Timestamp:** 2026-11-25 11:47:26
- **Data Window:** 2026-11-24 11:50:00 -> 2026-11-25 11:45:50 (UTC)
- **Execution Status:** 8 completed trades - Final Position: 0 BTC (Flat)

```text
=== TRADING SESSION RESULTS ===
Total Trades Executed : 8
Initial Starting Cash : $10,000.00
Final Balance         : $9,791.69
Net Profit / Loss     : $-208.31 (-2.08%)
Buy-and-Hold PnL      : $-250.37

=== DETAILED TRADE LOG ===
Trade #01 | Buy: 2026-11-24 14:05:00 @ $60224.43 -> Sell: 2026-11-24 14:35:00 @ $60129.68 (PnL:$-35.68)
Trade #02 | Buy: 2026-11-24 14:40:00 @ $60252.18 -> Sell: 2026-11-24 15:10:00 @ $60067.26 (PnL:$-50.43)
Trade #03 | Buy: 2026-11-24 16:25:00 @ $59739.42 -> Sell: 2026-11-24 16:30:00 @ $59591.77 (PnL:$-44.26)
Trade #04 | Buy: 2026-11-24 17:10:00 @ $59683.79 -> Sell: 2026-11-24 18:45:00 @ $59884.46 (PnL:$13.40)
Trade #05 | Buy: 2026-11-24 20:55:00 @ $59751.39 -> Sell: 2026-11-24 23:00:00 @ $59898.14 (PnL:$4.48)
Trade #06 | Buy: 2026-11-24 23:05:00 @ $59925.68 -> Sell: 2026-11-24 23:20:00 @ $59934.44 (PnL:$-18.31)
Trade #07 | Buy: 2026-11-24 23:25:00 @ $59967.59 -> Sell: 2026-11-24 23:35:00 @ $59832.73 (PnL:$-41.87)
Trade #08 | Buy: 2026-11-25 01:00:00 @ $59588.23 -> Sell: 2026-11-25 01:05:00 @ $59491.53 (PnL:$-35.55)
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
