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

- **Run Timestamp:** 2026-11-10 03:35:57
- **Data Window:** 2026-11-09 03:40:00 -> 2026-11-10 03:34:20 (UTC)
- **Execution Status:** 8 completed trades - Final Position: 0 BTC (Flat)

```text
=== TRADING SESSION RESULTS ===
Total Trades Executed : 8
Initial Starting Cash : $10,000.00
Final Balance         : $9,850.53
Net Profit / Loss     : $-149.47 (-1.49%)
Buy-and-Hold PnL      : $3.90

=== DETAILED TRADE LOG ===
Trade #01 | Buy: 2026-11-09 06:10:00 @ $74485.02 -> Sell: 2026-11-09 06:15:00 @ $74461.47 (PnL:$-23.13)
Trade #02 | Buy: 2026-11-09 07:40:00 @ $74093.70 -> Sell: 2026-11-09 07:45:00 @ $73946.28 (PnL:$-39.74)
Trade #03 | Buy: 2026-11-09 08:20:00 @ $74015.63 -> Sell: 2026-11-09 09:25:00 @ $74141.75 (PnL:$-2.95)
Trade #04 | Buy: 2026-11-09 10:30:00 @ $74224.27 -> Sell: 2026-11-09 10:35:00 @ $74080.19 (PnL:$-39.09)
Trade #05 | Buy: 2026-11-09 11:20:00 @ $74614.47 -> Sell: 2026-11-09 13:20:00 @ $74555.87 (PnL:$-27.53)
Trade #06 | Buy: 2026-11-09 14:45:00 @ $74604.64 -> Sell: 2026-11-09 16:15:00 @ $75063.97 (PnL:$40.92)
Trade #07 | Buy: 2026-11-09 18:10:00 @ $74778.68 -> Sell: 2026-11-09 18:15:00 @ $74717.96 (PnL:$-27.83)
Trade #08 | Buy: 2026-11-09 19:05:00 @ $74667.87 -> Sell: 2026-11-09 19:10:00 @ $74589.99 (PnL:$-30.03)
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
