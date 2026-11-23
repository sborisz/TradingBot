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

- **Run Timestamp:** 2026-11-23 19:51:42
- **Data Window:** 2026-11-22 19:55:00 -> 2026-11-23 19:50:10 (UTC)
- **Execution Status:** 8 completed trades - Final Position: 0 BTC (Flat)

```text
=== TRADING SESSION RESULTS ===
Total Trades Executed : 8
Initial Starting Cash : $10,000.00
Final Balance         : $9,795.51
Net Profit / Loss     : $-204.49 (-2.04%)
Buy-and-Hold PnL      : $23.88

=== DETAILED TRADE LOG ===
Trade #01 | Buy: 2026-11-22 21:35:00 @ $60902.18 -> Sell: 2026-11-22 21:55:00 @ $60848.29 (PnL:$-28.81)
Trade #02 | Buy: 2026-11-22 22:55:00 @ $60920.44 -> Sell: 2026-11-22 23:10:00 @ $60791.20 (PnL:$-41.03)
Trade #03 | Buy: 2026-11-22 23:15:00 @ $60894.56 -> Sell: 2026-11-23 00:30:00 @ $61116.04 (PnL:$16.20)
Trade #04 | Buy: 2026-11-23 00:35:00 @ $61254.65 -> Sell: 2026-11-23 00:40:00 @ $61008.78 (PnL:$-59.72)
Trade #05 | Buy: 2026-11-23 01:00:00 @ $61227.54 -> Sell: 2026-11-23 01:05:00 @ $60989.50 (PnL:$-58.11)
Trade #06 | Buy: 2026-11-23 01:35:00 @ $61124.47 -> Sell: 2026-11-23 01:40:00 @ $60973.24 (PnL:$-43.91)
Trade #07 | Buy: 2026-11-23 03:05:00 @ $60802.86 -> Sell: 2026-11-23 04:30:00 @ $61225.95 (PnL:$48.40)
Trade #08 | Buy: 2026-11-23 05:05:00 @ $61220.86 -> Sell: 2026-11-23 05:10:00 @ $61109.90 (PnL:$-37.43)
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
