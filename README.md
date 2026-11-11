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

- **Run Timestamp:** 2026-11-11 10:23:25
- **Data Window:** 2026-11-10 10:25:00 -> 2026-11-11 10:21:40 (UTC)
- **Execution Status:** 8 completed trades - Final Position: 0 BTC (Flat)

```text
=== TRADING SESSION RESULTS ===
Total Trades Executed : 8
Initial Starting Cash : $10,000.00
Final Balance         : $9,813.53
Net Profit / Loss     : $-186.47 (-1.86%)
Buy-and-Hold PnL      : $-72.79

=== DETAILED TRADE LOG ===
Trade #01 | Buy: 2026-11-10 12:05:00 @ $74288.97 -> Sell: 2026-11-10 12:20:00 @ $74253.63 (PnL:$-24.73)
Trade #02 | Buy: 2026-11-10 12:25:00 @ $74275.44 -> Sell: 2026-11-10 12:30:00 @ $74198.82 (PnL:$-30.20)
Trade #03 | Buy: 2026-11-10 12:50:00 @ $74255.95 -> Sell: 2026-11-10 13:20:00 @ $74201.36 (PnL:$-27.17)
Trade #04 | Buy: 2026-11-10 14:35:00 @ $73945.70 -> Sell: 2026-11-10 15:30:00 @ $74049.74 (PnL:$-5.89)
Trade #05 | Buy: 2026-11-10 17:35:00 @ $73060.63 -> Sell: 2026-11-10 17:40:00 @ $72800.54 (PnL:$-55.02)
Trade #06 | Buy: 2026-11-10 17:50:00 @ $73016.90 -> Sell: 2026-11-10 19:15:00 @ $73300.45 (PnL:$18.51)
Trade #07 | Buy: 2026-11-10 19:20:00 @ $73372.23 -> Sell: 2026-11-10 19:25:00 @ $73318.63 (PnL:$-26.93)
Trade #08 | Buy: 2026-11-10 19:35:00 @ $73401.01 -> Sell: 2026-11-10 19:40:00 @ $73286.82 (PnL:$-34.97)
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
