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

- **Run Timestamp:** 2026-11-11 06:49:16
- **Data Window:** 2026-11-10 06:50:00 -> 2026-11-11 06:47:40 (UTC)
- **Execution Status:** 8 completed trades - Final Position: 0 BTC (Flat)

```text
=== TRADING SESSION RESULTS ===
Total Trades Executed : 8
Initial Starting Cash : $10,000.00
Final Balance         : $9,780.39
Net Profit / Loss     : $-219.61 (-2.20%)
Buy-and-Hold PnL      : $-134.38

=== DETAILED TRADE LOG ===
Trade #01 | Buy: 2026-11-10 09:50:00 @ $74405.75 -> Sell: 2026-11-10 10:10:00 @ $74349.61 (PnL:$-27.51)
Trade #02 | Buy: 2026-11-10 11:15:00 @ $74286.85 -> Sell: 2026-11-10 11:20:00 @ $74176.33 (PnL:$-34.73)
Trade #03 | Buy: 2026-11-10 11:30:00 @ $74223.55 -> Sell: 2026-11-10 11:35:00 @ $74185.34 (PnL:$-24.96)
Trade #04 | Buy: 2026-11-10 11:40:00 @ $74217.38 -> Sell: 2026-11-10 12:20:00 @ $74253.63 (PnL:$-14.97)
Trade #05 | Buy: 2026-11-10 12:25:00 @ $74275.44 -> Sell: 2026-11-10 12:30:00 @ $74198.82 (PnL:$-29.97)
Trade #06 | Buy: 2026-11-10 12:50:00 @ $74255.95 -> Sell: 2026-11-10 13:20:00 @ $74201.36 (PnL:$-26.96)
Trade #07 | Buy: 2026-11-10 14:35:00 @ $73945.70 -> Sell: 2026-11-10 15:30:00 @ $74049.74 (PnL:$-5.84)
Trade #08 | Buy: 2026-11-10 17:35:00 @ $73060.63 -> Sell: 2026-11-10 17:40:00 @ $72800.54 (PnL:$-54.59)
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
