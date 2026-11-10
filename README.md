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

- **Run Timestamp:** 2026-11-10 22:08:52
- **Data Window:** 2026-11-09 22:10:00 -> 2026-11-10 22:07:10 (UTC)
- **Execution Status:** 8 completed trades - Final Position: 0 BTC (Flat)

```text
=== TRADING SESSION RESULTS ===
Total Trades Executed : 8
Initial Starting Cash : $10,000.00
Final Balance         : $9,801.99
Net Profit / Loss     : $-198.01 (-1.98%)
Buy-and-Hold PnL      : $-176.40

=== DETAILED TRADE LOG ===
Trade #01 | Buy: 2026-11-09 23:50:00 @ $75163.45 -> Sell: 2026-11-10 00:20:00 @ $75095.92 (PnL:$-28.95)
Trade #02 | Buy: 2026-11-10 01:05:00 @ $75123.72 -> Sell: 2026-11-10 01:20:00 @ $75059.08 (PnL:$-28.48)
Trade #03 | Buy: 2026-11-10 01:30:00 @ $75065.69 -> Sell: 2026-11-10 01:35:00 @ $74895.79 (PnL:$-42.32)
Trade #04 | Buy: 2026-11-10 04:20:00 @ $74605.14 -> Sell: 2026-11-10 05:35:00 @ $74682.94 (PnL:$-9.48)
Trade #05 | Buy: 2026-11-10 05:40:00 @ $74767.75 -> Sell: 2026-11-10 06:05:00 @ $74693.24 (PnL:$-29.60)
Trade #06 | Buy: 2026-11-10 07:05:00 @ $74649.41 -> Sell: 2026-11-10 07:10:00 @ $74594.15 (PnL:$-26.99)
Trade #07 | Buy: 2026-11-10 07:15:00 @ $74607.70 -> Sell: 2026-11-10 08:10:00 @ $74718.48 (PnL:$-5.08)
Trade #08 | Buy: 2026-11-10 09:50:00 @ $74405.75 -> Sell: 2026-11-10 10:10:00 @ $74349.61 (PnL:$-27.04)
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
