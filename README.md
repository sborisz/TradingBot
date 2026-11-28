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

- **Run Timestamp:** 2026-11-28 17:12:57
- **Data Window:** 2026-11-27 17:15:00 -> 2026-11-28 17:11:20 (UTC)
- **Execution Status:** 8 completed trades - Final Position: 0 BTC (Flat)

```text
=== TRADING SESSION RESULTS ===
Total Trades Executed : 8
Initial Starting Cash : $10,000.00
Final Balance         : $9,827.55
Net Profit / Loss     : $-172.45 (-1.72%)
Buy-and-Hold PnL      : $-52.33

=== DETAILED TRADE LOG ===
Trade #01 | Buy: 2026-11-27 18:55:00 @ $59861.79 -> Sell: 2026-11-27 19:10:00 @ $59814.78 (PnL:$-27.82)
Trade #02 | Buy: 2026-11-27 19:20:00 @ $59922.37 -> Sell: 2026-11-27 19:55:00 @ $59833.09 (PnL:$-34.75)
Trade #03 | Buy: 2026-11-27 21:10:00 @ $59774.06 -> Sell: 2026-11-27 21:35:00 @ $59744.97 (PnL:$-24.68)
Trade #04 | Buy: 2026-11-27 21:40:00 @ $59811.93 -> Sell: 2026-11-27 21:50:00 @ $59758.88 (PnL:$-28.58)
Trade #05 | Buy: 2026-11-27 22:50:00 @ $59777.12 -> Sell: 2026-11-27 23:00:00 @ $59705.96 (PnL:$-31.49)
Trade #06 | Buy: 2026-11-27 23:05:00 @ $59789.57 -> Sell: 2026-11-28 00:55:00 @ $59891.04 (PnL:$-3.00)
Trade #07 | Buy: 2026-11-28 01:05:00 @ $59949.85 -> Sell: 2026-11-28 02:30:00 @ $60093.75 (PnL:$3.92)
Trade #08 | Buy: 2026-11-28 03:45:00 @ $59924.95 -> Sell: 2026-11-28 03:50:00 @ $59886.71 (PnL:$-25.96)
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
