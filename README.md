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

- **Run Timestamp:** 2026-11-14 06:38:15
- **Data Window:** 2026-11-13 06:40:00 -> 2026-11-14 06:36:50 (UTC)
- **Execution Status:** 8 completed trades - Final Position: 0 BTC (Flat)

```text
=== TRADING SESSION RESULTS ===
Total Trades Executed : 8
Initial Starting Cash : $10,000.00
Final Balance         : $9,808.15
Net Profit / Loss     : $-191.85 (-1.92%)
Buy-and-Hold PnL      : $-181.78

=== DETAILED TRADE LOG ===
Trade #01 | Buy: 2026-11-13 08:40:00 @ $68764.79 -> Sell: 2026-11-13 09:00:00 @ $68653.22 (PnL:$-36.17)
Trade #02 | Buy: 2026-11-13 09:05:00 @ $68834.39 -> Sell: 2026-11-13 09:10:00 @ $68634.04 (PnL:$-48.85)
Trade #03 | Buy: 2026-11-13 09:15:00 @ $68752.75 -> Sell: 2026-11-13 11:15:00 @ $69259.41 (PnL:$53.11)
Trade #04 | Buy: 2026-11-13 11:20:00 @ $69356.68 -> Sell: 2026-11-13 11:30:00 @ $69229.79 (PnL:$-38.12)
Trade #05 | Buy: 2026-11-13 11:45:00 @ $69287.78 -> Sell: 2026-11-13 12:25:00 @ $69230.32 (PnL:$-28.06)
Trade #06 | Buy: 2026-11-13 12:35:00 @ $69296.62 -> Sell: 2026-11-13 12:40:00 @ $69215.63 (PnL:$-31.33)
Trade #07 | Buy: 2026-11-13 13:30:00 @ $69264.74 -> Sell: 2026-11-13 14:05:00 @ $69394.45 (PnL:$-1.27)
Trade #08 | Buy: 2026-11-13 16:00:00 @ $68807.00 -> Sell: 2026-11-13 16:05:00 @ $68518.11 (PnL:$-61.07)
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
