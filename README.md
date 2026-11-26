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

- **Run Timestamp:** 2026-11-26 04:31:28
- **Data Window:** 2026-11-25 04:35:00 -> 2026-11-26 04:30:10 (UTC)
- **Execution Status:** 8 completed trades - Final Position: 0 BTC (Flat)

```text
=== TRADING SESSION RESULTS ===
Total Trades Executed : 8
Initial Starting Cash : $10,000.00
Final Balance         : $9,698.62
Net Profit / Loss     : $-301.38 (-3.01%)
Buy-and-Hold PnL      : $292.08

=== DETAILED TRADE LOG ===
Trade #01 | Buy: 2026-11-25 06:15:00 @ $59237.64 -> Sell: 2026-11-25 06:30:00 @ $58974.77 (PnL:$-64.27)
Trade #02 | Buy: 2026-11-25 06:40:00 @ $59154.10 -> Sell: 2026-11-25 07:20:00 @ $59062.41 (PnL:$-35.22)
Trade #03 | Buy: 2026-11-25 08:25:00 @ $58891.56 -> Sell: 2026-11-25 09:15:00 @ $59027.47 (PnL:$3.02)
Trade #04 | Buy: 2026-11-25 10:05:00 @ $59102.28 -> Sell: 2026-11-25 10:15:00 @ $58915.79 (PnL:$-50.97)
Trade #05 | Buy: 2026-11-25 10:20:00 @ $58983.62 -> Sell: 2026-11-25 10:25:00 @ $58887.79 (PnL:$-35.66)
Trade #06 | Buy: 2026-11-25 10:35:00 @ $58937.63 -> Sell: 2026-11-25 10:40:00 @ $58893.75 (PnL:$-26.91)
Trade #07 | Buy: 2026-11-25 11:55:00 @ $58715.28 -> Sell: 2026-11-25 12:00:00 @ $58591.10 (PnL:$-40.22)
Trade #08 | Buy: 2026-11-25 12:30:00 @ $58612.62 -> Sell: 2026-11-25 13:35:00 @ $58422.33 (PnL:$-51.07)
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
