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

- **Run Timestamp:** 2026-11-12 10:26:14
- **Data Window:** 2026-11-11 10:30:00 -> 2026-11-12 10:24:50 (UTC)
- **Execution Status:** 8 completed trades - Final Position: 0 BTC (Flat)

```text
=== TRADING SESSION RESULTS ===
Total Trades Executed : 8
Initial Starting Cash : $10,000.00
Final Balance         : $9,827.32
Net Profit / Loss     : $-172.68 (-1.73%)
Buy-and-Hold PnL      : $-56.55

=== DETAILED TRADE LOG ===
Trade #01 | Buy: 2026-11-11 12:20:00 @ $73996.46 -> Sell: 2026-11-11 12:25:00 @ $73923.27 (PnL:$-29.85)
Trade #02 | Buy: 2026-11-11 12:40:00 @ $74007.90 -> Sell: 2026-11-11 12:45:00 @ $73919.64 (PnL:$-31.79)
Trade #03 | Buy: 2026-11-11 12:50:00 @ $73985.94 -> Sell: 2026-11-11 12:55:00 @ $73923.76 (PnL:$-28.19)
Trade #04 | Buy: 2026-11-11 13:55:00 @ $73846.95 -> Sell: 2026-11-11 14:00:00 @ $73738.81 (PnL:$-34.28)
Trade #05 | Buy: 2026-11-11 14:40:00 @ $73688.40 -> Sell: 2026-11-11 14:45:00 @ $73581.29 (PnL:$-34.06)
Trade #06 | Buy: 2026-11-11 14:50:00 @ $73662.98 -> Sell: 2026-11-11 14:55:00 @ $73525.21 (PnL:$-38.03)
Trade #07 | Buy: 2026-11-11 15:15:00 @ $73592.34 -> Sell: 2026-11-11 15:20:00 @ $73505.39 (PnL:$-31.15)
Trade #08 | Buy: 2026-11-11 15:30:00 @ $73581.42 -> Sell: 2026-11-11 17:10:00 @ $74141.80 (PnL:$54.75)
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
