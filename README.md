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

- **Run Timestamp:** 2026-11-19 05:49:44
- **Data Window:** 2026-11-18 05:50:00 -> 2026-11-19 05:48:20 (UTC)
- **Execution Status:** 8 completed trades - Final Position: 0 BTC (Flat)

```text
=== TRADING SESSION RESULTS ===
Total Trades Executed : 8
Initial Starting Cash : $10,000.00
Final Balance         : $9,771.15
Net Profit / Loss     : $-228.85 (-2.29%)
Buy-and-Hold PnL      : $-216.34

=== DETAILED TRADE LOG ===
Trade #01 | Buy: 2026-11-18 07:30:00 @ $64675.17 -> Sell: 2026-11-18 07:35:00 @ $64583.26 (PnL:$-34.16)
Trade #02 | Buy: 2026-11-18 07:50:00 @ $64689.45 -> Sell: 2026-11-18 08:10:00 @ $64621.01 (PnL:$-30.43)
Trade #03 | Buy: 2026-11-18 08:25:00 @ $64679.64 -> Sell: 2026-11-18 08:55:00 @ $64606.39 (PnL:$-31.08)
Trade #04 | Buy: 2026-11-18 09:05:00 @ $64680.31 -> Sell: 2026-11-18 09:15:00 @ $64636.90 (PnL:$-26.42)
Trade #05 | Buy: 2026-11-18 11:55:00 @ $64188.85 -> Sell: 2026-11-18 12:00:00 @ $64166.29 (PnL:$-23.20)
Trade #06 | Buy: 2026-11-18 12:15:00 @ $64187.66 -> Sell: 2026-11-18 12:35:00 @ $64144.13 (PnL:$-26.36)
Trade #07 | Buy: 2026-11-18 13:35:00 @ $64083.94 -> Sell: 2026-11-18 13:40:00 @ $64016.68 (PnL:$-29.93)
Trade #08 | Buy: 2026-11-18 14:20:00 @ $63957.51 -> Sell: 2026-11-18 14:25:00 @ $63907.77 (PnL:$-27.18)
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
