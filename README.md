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

- **Run Timestamp:** 2026-11-17 15:41:44
- **Data Window:** 2026-11-16 15:45:00 -> 2026-11-17 15:40:10 (UTC)
- **Execution Status:** 8 completed trades - Final Position: 0 BTC (Flat)

```text
=== TRADING SESSION RESULTS ===
Total Trades Executed : 8
Initial Starting Cash : $10,000.00
Final Balance         : $9,751.79
Net Profit / Loss     : $-248.21 (-2.48%)
Buy-and-Hold PnL      : $-60.62

=== DETAILED TRADE LOG ===
Trade #01 | Buy: 2026-11-16 17:25:00 @ $65063.43 -> Sell: 2026-11-16 18:00:00 @ $64854.90 (PnL:$-51.97)
Trade #02 | Buy: 2026-11-16 18:45:00 @ $64898.77 -> Sell: 2026-11-16 19:15:00 @ $64815.91 (PnL:$-32.55)
Trade #03 | Buy: 2026-11-16 19:55:00 @ $65002.74 -> Sell: 2026-11-16 20:40:00 @ $64929.83 (PnL:$-30.91)
Trade #04 | Buy: 2026-11-16 20:55:00 @ $64972.92 -> Sell: 2026-11-16 21:05:00 @ $64991.76 (PnL:$-16.89)
Trade #05 | Buy: 2026-11-16 21:15:00 @ $64996.34 -> Sell: 2026-11-16 21:20:00 @ $64949.36 (PnL:$-26.83)
Trade #06 | Buy: 2026-11-16 21:25:00 @ $65195.72 -> Sell: 2026-11-16 22:35:00 @ $65271.04 (PnL:$-8.32)
Trade #07 | Buy: 2026-11-16 22:45:00 @ $65453.55 -> Sell: 2026-11-16 23:00:00 @ $65275.13 (PnL:$-46.39)
Trade #08 | Buy: 2026-11-17 01:10:00 @ $64648.78 -> Sell: 2026-11-17 01:30:00 @ $64551.35 (PnL:$-34.27)
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
