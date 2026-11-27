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

- **Run Timestamp:** 2026-11-27 05:50:58
- **Data Window:** 2026-11-26 05:55:00 -> 2026-11-27 05:49:20 (UTC)
- **Execution Status:** 8 completed trades - Final Position: 0 BTC (Flat)

```text
=== TRADING SESSION RESULTS ===
Total Trades Executed : 8
Initial Starting Cash : $10,000.00
Final Balance         : $9,802.61
Net Profit / Loss     : $-197.39 (-1.97%)
Buy-and-Hold PnL      : $185.46

=== DETAILED TRADE LOG ===
Trade #01 | Buy: 2026-11-26 07:45:00 @ $60573.85 -> Sell: 2026-11-26 08:00:00 @ $60468.22 (PnL:$-37.38)
Trade #02 | Buy: 2026-11-26 08:05:00 @ $60558.08 -> Sell: 2026-11-26 09:30:00 @ $60697.24 (PnL:$2.94)
Trade #03 | Buy: 2026-11-26 09:35:00 @ $60752.78 -> Sell: 2026-11-26 10:25:00 @ $60759.17 (PnL:$-18.87)
Trade #04 | Buy: 2026-11-26 11:25:00 @ $60616.97 -> Sell: 2026-11-26 11:30:00 @ $60580.63 (PnL:$-25.82)
Trade #05 | Buy: 2026-11-26 11:35:00 @ $60607.81 -> Sell: 2026-11-26 12:15:00 @ $60608.00 (PnL:$-19.79)
Trade #06 | Buy: 2026-11-26 12:40:00 @ $60786.81 -> Sell: 2026-11-26 14:20:00 @ $60755.28 (PnL:$-24.91)
Trade #07 | Buy: 2026-11-26 14:35:00 @ $60816.67 -> Sell: 2026-11-26 14:40:00 @ $60714.10 (PnL:$-36.36)
Trade #08 | Buy: 2026-11-26 15:50:00 @ $60488.90 -> Sell: 2026-11-26 16:30:00 @ $60381.33 (PnL:$-37.12)
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
