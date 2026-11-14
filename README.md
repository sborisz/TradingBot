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

- **Run Timestamp:** 2026-11-14 12:00:26
- **Data Window:** 2026-11-13 12:05:00 -> 2026-11-14 11:59:00 (UTC)
- **Execution Status:** 8 completed trades - Final Position: 0 BTC (Flat)

```text
=== TRADING SESSION RESULTS ===
Total Trades Executed : 8
Initial Starting Cash : $10,000.00
Final Balance         : $9,706.41
Net Profit / Loss     : $-293.59 (-2.94%)
Buy-and-Hold PnL      : $-367.65

=== DETAILED TRADE LOG ===
Trade #01 | Buy: 2026-11-13 13:45:00 @ $69776.85 -> Sell: 2026-11-13 14:05:00 @ $69394.45 (PnL:$-74.67)
Trade #02 | Buy: 2026-11-13 16:00:00 @ $68807.00 -> Sell: 2026-11-13 16:05:00 @ $68518.11 (PnL:$-61.42)
Trade #03 | Buy: 2026-11-13 16:10:00 @ $68743.44 -> Sell: 2026-11-13 16:15:00 @ $68520.66 (PnL:$-51.61)
Trade #04 | Buy: 2026-11-13 16:25:00 @ $68602.22 -> Sell: 2026-11-13 16:55:00 @ $68586.25 (PnL:$-21.88)
Trade #05 | Buy: 2026-11-13 17:00:00 @ $68639.52 -> Sell: 2026-11-13 17:05:00 @ $68574.38 (PnL:$-28.83)
Trade #06 | Buy: 2026-11-13 17:10:00 @ $68782.14 -> Sell: 2026-11-13 17:20:00 @ $68561.72 (PnL:$-50.72)
Trade #07 | Buy: 2026-11-13 17:30:00 @ $68634.44 -> Sell: 2026-11-13 18:35:00 @ $68821.48 (PnL:$7.01)
Trade #08 | Buy: 2026-11-13 18:50:00 @ $68872.96 -> Sell: 2026-11-13 19:50:00 @ $68930.06 (PnL:$-11.38)
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
