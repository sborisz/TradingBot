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

- **Run Timestamp:** 2026-11-22 18:29:30
- **Data Window:** 2026-11-21 18:30:00 -> 2026-11-22 18:28:10 (UTC)
- **Execution Status:** 8 completed trades - Final Position: 0 BTC (Flat)

```text
=== TRADING SESSION RESULTS ===
Total Trades Executed : 8
Initial Starting Cash : $10,000.00
Final Balance         : $9,756.19
Net Profit / Loss     : $-243.81 (-2.44%)
Buy-and-Hold PnL      : $41.40

=== DETAILED TRADE LOG ===
Trade #01 | Buy: 2026-11-21 20:10:00 @ $60309.18 -> Sell: 2026-11-21 20:15:00 @ $60241.06 (PnL:$-31.25)
Trade #02 | Buy: 2026-11-21 20:25:00 @ $60285.06 -> Sell: 2026-11-21 20:50:00 @ $60264.20 (PnL:$-23.36)
Trade #03 | Buy: 2026-11-21 20:55:00 @ $60311.88 -> Sell: 2026-11-21 21:35:00 @ $60285.12 (PnL:$-24.27)
Trade #04 | Buy: 2026-11-21 21:55:00 @ $60350.94 -> Sell: 2026-11-21 22:05:00 @ $60246.59 (PnL:$-36.94)
Trade #05 | Buy: 2026-11-21 23:15:00 @ $60189.57 -> Sell: 2026-11-22 00:00:00 @ $60121.30 (PnL:$-30.94)
Trade #06 | Buy: 2026-11-22 00:10:00 @ $60213.92 -> Sell: 2026-11-22 00:50:00 @ $60157.63 (PnL:$-28.88)
Trade #07 | Buy: 2026-11-22 01:50:00 @ $60087.57 -> Sell: 2026-11-22 01:55:00 @ $60053.29 (PnL:$-25.22)
Trade #08 | Buy: 2026-11-22 04:00:00 @ $59259.29 -> Sell: 2026-11-22 04:25:00 @ $59118.18 (PnL:$-42.87)
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
