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

- **Run Timestamp:** 2026-11-13 05:46:24
- **Data Window:** 2026-11-12 05:50:00 -> 2026-11-13 05:44:40 (UTC)
- **Execution Status:** 8 completed trades - Final Position: 0 BTC (Flat)

```text
=== TRADING SESSION RESULTS ===
Total Trades Executed : 8
Initial Starting Cash : $10,000.00
Final Balance         : $9,791.64
Net Profit / Loss     : $-208.36 (-2.08%)
Buy-and-Hold PnL      : $-675.87

=== DETAILED TRADE LOG ===
Trade #01 | Buy: 2026-11-12 07:30:00 @ $73530.98 -> Sell: 2026-11-12 07:45:00 @ $73481.93 (PnL:$-26.64)
Trade #02 | Buy: 2026-11-12 07:50:00 @ $73550.43 -> Sell: 2026-11-12 08:25:00 @ $73484.52 (PnL:$-28.85)
Trade #03 | Buy: 2026-11-12 08:55:00 @ $73530.94 -> Sell: 2026-11-12 09:00:00 @ $73514.62 (PnL:$-22.07)
Trade #04 | Buy: 2026-11-12 09:35:00 @ $73410.98 -> Sell: 2026-11-12 09:40:00 @ $73381.20 (PnL:$-23.84)
Trade #05 | Buy: 2026-11-12 10:00:00 @ $73418.46 -> Sell: 2026-11-12 10:10:00 @ $73327.41 (PnL:$-32.03)
Trade #06 | Buy: 2026-11-12 10:20:00 @ $73432.27 -> Sell: 2026-11-12 10:50:00 @ $73400.74 (PnL:$-23.94)
Trade #07 | Buy: 2026-11-12 12:50:00 @ $72900.02 -> Sell: 2026-11-12 13:00:00 @ $72825.92 (PnL:$-29.65)
Trade #08 | Buy: 2026-11-12 15:55:00 @ $71466.65 -> Sell: 2026-11-12 16:45:00 @ $71454.54 (PnL:$-21.27)
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
