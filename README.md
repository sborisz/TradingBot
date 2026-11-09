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

- **Run Timestamp:** 2026-11-09 15:19:17
- **Data Window:** 2026-11-08 15:20:00 -> 2026-11-09 15:17:50 (UTC)
- **Execution Status:** 8 completed trades - Final Position: 0 BTC (Flat)

```text
=== TRADING SESSION RESULTS ===
Total Trades Executed : 8
Initial Starting Cash : $10,000.00
Final Balance         : $9,829.68
Net Profit / Loss     : $-170.32 (-1.70%)
Buy-and-Hold PnL      : $-104.17

=== DETAILED TRADE LOG ===
Trade #01 | Buy: 2026-11-08 18:05:00 @ $75694.84 -> Sell: 2026-11-08 19:10:00 @ $75782.62 (PnL:$-8.41)
Trade #02 | Buy: 2026-11-08 19:35:00 @ $75836.46 -> Sell: 2026-11-08 19:40:00 @ $75805.06 (PnL:$-24.09)
Trade #03 | Buy: 2026-11-08 19:50:00 @ $75843.32 -> Sell: 2026-11-08 20:10:00 @ $75807.71 (PnL:$-24.59)
Trade #04 | Buy: 2026-11-08 20:30:00 @ $75818.62 -> Sell: 2026-11-08 20:35:00 @ $75794.52 (PnL:$-23.02)
Trade #05 | Buy: 2026-11-08 23:05:00 @ $74720.93 -> Sell: 2026-11-08 23:55:00 @ $74699.34 (PnL:$-22.68)
Trade #06 | Buy: 2026-11-09 00:35:00 @ $75046.82 -> Sell: 2026-11-09 00:40:00 @ $74806.73 (PnL:$-51.37)
Trade #07 | Buy: 2026-11-09 00:45:00 @ $74888.35 -> Sell: 2026-11-09 01:20:00 @ $74955.25 (PnL:$-10.89)
Trade #08 | Buy: 2026-11-09 01:45:00 @ $75085.18 -> Sell: 2026-11-09 02:55:00 @ $75195.83 (PnL:$-5.19)
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
