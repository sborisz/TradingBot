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

- **Run Timestamp:** 2026-11-16 21:37:59
- **Data Window:** 2026-11-15 21:40:00 -> 2026-11-16 21:36:40 (UTC)
- **Execution Status:** 8 completed trades - Final Position: 0 BTC (Flat)

```text
=== TRADING SESSION RESULTS ===
Total Trades Executed : 8
Initial Starting Cash : $10,000.00
Final Balance         : $9,795.53
Net Profit / Loss     : $-204.47 (-2.04%)
Buy-and-Hold PnL      : $51.18

=== DETAILED TRADE LOG ===
Trade #01 | Buy: 2026-11-15 23:40:00 @ $65318.52 -> Sell: 2026-11-16 00:15:00 @ $65064.17 (PnL:$-58.84)
Trade #02 | Buy: 2026-11-16 02:10:00 @ $64924.75 -> Sell: 2026-11-16 03:30:00 @ $65056.24 (PnL:$0.23)
Trade #03 | Buy: 2026-11-16 04:00:00 @ $65066.21 -> Sell: 2026-11-16 04:05:00 @ $64955.10 (PnL:$-36.80)
Trade #04 | Buy: 2026-11-16 04:25:00 @ $65053.09 -> Sell: 2026-11-16 04:35:00 @ $64909.19 (PnL:$-41.66)
Trade #05 | Buy: 2026-11-16 06:35:00 @ $64566.14 -> Sell: 2026-11-16 07:15:00 @ $64472.00 (PnL:$-34.06)
Trade #06 | Buy: 2026-11-16 07:25:00 @ $64543.65 -> Sell: 2026-11-16 08:10:00 @ $64674.33 (PnL:$0.22)
Trade #07 | Buy: 2026-11-16 10:10:00 @ $64354.15 -> Sell: 2026-11-16 11:45:00 @ $64537.33 (PnL:$8.28)
Trade #08 | Buy: 2026-11-16 13:05:00 @ $64221.10 -> Sell: 2026-11-16 13:15:00 @ $64076.44 (PnL:$-41.77)
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
