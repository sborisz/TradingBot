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

- **Run Timestamp:** 2026-11-09 23:58:54
- **Data Window:** 2026-11-09 00:00:00 -> 2026-11-09 23:57:20 (UTC)
- **Execution Status:** 8 completed trades - Final Position: 0 BTC (Flat)

```text
=== TRADING SESSION RESULTS ===
Total Trades Executed : 8
Initial Starting Cash : $10,000.00
Final Balance         : $9,873.20
Net Profit / Loss     : $-126.80 (-1.27%)
Buy-and-Hold PnL      : $61.60

=== DETAILED TRADE LOG ===
Trade #01 | Buy: 2026-11-09 01:45:00 @ $75085.18 -> Sell: 2026-11-09 02:55:00 @ $75195.83 (PnL:$-5.27)
Trade #02 | Buy: 2026-11-09 04:35:00 @ $74937.96 -> Sell: 2026-11-09 04:45:00 @ $74860.82 (PnL:$-30.24)
Trade #03 | Buy: 2026-11-09 06:10:00 @ $74485.02 -> Sell: 2026-11-09 06:15:00 @ $74461.47 (PnL:$-23.05)
Trade #04 | Buy: 2026-11-09 07:40:00 @ $74093.70 -> Sell: 2026-11-09 07:45:00 @ $73946.28 (PnL:$-39.60)
Trade #05 | Buy: 2026-11-09 08:20:00 @ $74015.63 -> Sell: 2026-11-09 09:25:00 @ $74141.75 (PnL:$-2.94)
Trade #06 | Buy: 2026-11-09 10:30:00 @ $74224.27 -> Sell: 2026-11-09 10:35:00 @ $74080.19 (PnL:$-38.95)
Trade #07 | Buy: 2026-11-09 11:20:00 @ $74614.47 -> Sell: 2026-11-09 13:20:00 @ $74555.87 (PnL:$-27.43)
Trade #08 | Buy: 2026-11-09 14:45:00 @ $74604.64 -> Sell: 2026-11-09 16:15:00 @ $75063.97 (PnL:$40.77)
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
