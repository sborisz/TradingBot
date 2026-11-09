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

- **Run Timestamp:** 2026-11-09 00:39:50
- **Data Window:** 2026-11-08 00:40:00 -> 2026-11-09 00:38:10 (UTC)
- **Execution Status:** 8 completed trades - Final Position: 0 BTC (Flat)

```text
=== TRADING SESSION RESULTS ===
Total Trades Executed : 8
Initial Starting Cash : $10,000.00
Final Balance         : $9,853.74
Net Profit / Loss     : $-146.26 (-1.46%)
Buy-and-Hold PnL      : $-103.40

=== DETAILED TRADE LOG ===
Trade #01 | Buy: 2026-11-08 03:00:00 @ $75695.02 -> Sell: 2026-11-08 04:45:00 @ $75820.63 (PnL:$-3.42)
Trade #02 | Buy: 2026-11-08 05:15:00 @ $75822.94 -> Sell: 2026-11-08 05:20:00 @ $75811.03 (PnL:$-21.54)
Trade #03 | Buy: 2026-11-08 06:25:00 @ $75760.46 -> Sell: 2026-11-08 06:45:00 @ $75705.70 (PnL:$-27.13)
Trade #04 | Buy: 2026-11-08 07:00:00 @ $75749.96 -> Sell: 2026-11-08 08:20:00 @ $75803.96 (PnL:$-12.80)
Trade #05 | Buy: 2026-11-08 08:40:00 @ $75836.88 -> Sell: 2026-11-08 09:10:00 @ $75824.80 (PnL:$-21.43)
Trade #06 | Buy: 2026-11-08 09:40:00 @ $75842.72 -> Sell: 2026-11-08 10:05:00 @ $75819.61 (PnL:$-22.82)
Trade #07 | Buy: 2026-11-08 11:05:00 @ $75835.04 -> Sell: 2026-11-08 12:15:00 @ $75878.31 (PnL:$-14.13)
Trade #08 | Buy: 2026-11-08 12:30:00 @ $75898.84 -> Sell: 2026-11-08 12:40:00 @ $75874.33 (PnL:$-22.92)
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
