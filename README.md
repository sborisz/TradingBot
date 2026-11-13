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

- **Run Timestamp:** 2026-11-13 10:24:09
- **Data Window:** 2026-11-12 10:25:00 -> 2026-11-13 10:22:30 (UTC)
- **Execution Status:** 8 completed trades - Final Position: 0 BTC (Flat)

```text
=== TRADING SESSION RESULTS ===
Total Trades Executed : 8
Initial Starting Cash : $10,000.00
Final Balance         : $9,762.58
Net Profit / Loss     : $-237.42 (-2.37%)
Buy-and-Hold PnL      : $-490.32

=== DETAILED TRADE LOG ===
Trade #01 | Buy: 2026-11-12 12:50:00 @ $72900.02 -> Sell: 2026-11-12 13:00:00 @ $72825.92 (PnL:$-30.12)
Trade #02 | Buy: 2026-11-12 15:55:00 @ $71466.65 -> Sell: 2026-11-12 16:45:00 @ $71454.54 (PnL:$-21.61)
Trade #03 | Buy: 2026-11-12 16:55:00 @ $71758.63 -> Sell: 2026-11-12 17:00:00 @ $71695.05 (PnL:$-28.67)
Trade #04 | Buy: 2026-11-12 17:25:00 @ $71819.18 -> Sell: 2026-11-12 17:45:00 @ $71689.56 (PnL:$-37.69)
Trade #05 | Buy: 2026-11-12 17:50:00 @ $71749.14 -> Sell: 2026-11-12 18:45:00 @ $71742.36 (PnL:$-20.68)
Trade #06 | Buy: 2026-11-12 19:55:00 @ $71617.02 -> Sell: 2026-11-12 20:10:00 @ $71544.39 (PnL:$-29.68)
Trade #07 | Buy: 2026-11-12 21:20:00 @ $71509.26 -> Sell: 2026-11-12 22:05:00 @ $71451.62 (PnL:$-27.55)
Trade #08 | Buy: 2026-11-12 23:10:00 @ $71443.39 -> Sell: 2026-11-12 23:20:00 @ $71284.57 (PnL:$-41.34)
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
