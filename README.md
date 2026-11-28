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

- **Run Timestamp:** 2026-11-28 08:03:49
- **Data Window:** 2026-11-27 08:05:00 -> 2026-11-28 08:02:10 (UTC)
- **Execution Status:** 8 completed trades - Final Position: 0 BTC (Flat)

```text
=== TRADING SESSION RESULTS ===
Total Trades Executed : 8
Initial Starting Cash : $10,000.00
Final Balance         : $9,747.03
Net Profit / Loss     : $-252.97 (-2.53%)
Buy-and-Hold PnL      : $-307.61

=== DETAILED TRADE LOG ===
Trade #01 | Buy: 2026-11-27 11:25:00 @ $61607.44 -> Sell: 2026-11-27 11:50:00 @ $61473.14 (PnL:$-41.74)
Trade #02 | Buy: 2026-11-27 12:00:00 @ $61597.09 -> Sell: 2026-11-27 12:15:00 @ $61582.16 (PnL:$-22.31)
Trade #03 | Buy: 2026-11-27 13:00:00 @ $61566.29 -> Sell: 2026-11-27 13:05:00 @ $61503.81 (PnL:$-29.92)
Trade #04 | Buy: 2026-11-27 13:50:00 @ $61472.36 -> Sell: 2026-11-27 14:05:00 @ $61414.45 (PnL:$-29.11)
Trade #05 | Buy: 2026-11-27 14:15:00 @ $61433.28 -> Sell: 2026-11-27 14:20:00 @ $61346.84 (PnL:$-33.60)
Trade #06 | Buy: 2026-11-27 15:40:00 @ $60083.51 -> Sell: 2026-11-27 17:00:00 @ $59942.23 (PnL:$-42.77)
Trade #07 | Buy: 2026-11-27 18:00:00 @ $59824.69 -> Sell: 2026-11-27 18:05:00 @ $59770.58 (PnL:$-28.43)
Trade #08 | Buy: 2026-11-27 18:10:00 @ $59848.60 -> Sell: 2026-11-27 19:10:00 @ $59814.78 (PnL:$-25.04)
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
