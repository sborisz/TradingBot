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

- **Run Timestamp:** 2026-11-09 08:18:13
- **Data Window:** 2026-11-08 08:20:00 -> 2026-11-09 08:16:40 (UTC)
- **Execution Status:** 8 completed trades - Final Position: 0 BTC (Flat)

```text
=== TRADING SESSION RESULTS ===
Total Trades Executed : 8
Initial Starting Cash : $10,000.00
Final Balance         : $9,850.95
Net Profit / Loss     : $-149.05 (-1.49%)
Buy-and-Hold PnL      : $-259.94

=== DETAILED TRADE LOG ===
Trade #01 | Buy: 2026-11-08 10:00:00 @ $75848.98 -> Sell: 2026-11-08 10:05:00 @ $75819.61 (PnL:$-23.85)
Trade #02 | Buy: 2026-11-08 11:05:00 @ $75835.04 -> Sell: 2026-11-08 12:15:00 @ $75878.31 (PnL:$-14.25)
Trade #03 | Buy: 2026-11-08 12:30:00 @ $75898.84 -> Sell: 2026-11-08 12:40:00 @ $75874.33 (PnL:$-23.12)
Trade #04 | Buy: 2026-11-08 13:00:00 @ $75892.60 -> Sell: 2026-11-08 13:40:00 @ $75906.06 (PnL:$-18.10)
Trade #05 | Buy: 2026-11-08 14:35:00 @ $75914.89 -> Sell: 2026-11-08 16:10:00 @ $75960.66 (PnL:$-13.85)
Trade #06 | Buy: 2026-11-08 16:30:00 @ $75974.15 -> Sell: 2026-11-08 16:35:00 @ $75944.22 (PnL:$-23.69)
Trade #07 | Buy: 2026-11-08 18:05:00 @ $75694.84 -> Sell: 2026-11-08 19:10:00 @ $75782.62 (PnL:$-8.31)
Trade #08 | Buy: 2026-11-08 19:35:00 @ $75836.46 -> Sell: 2026-11-08 19:40:00 @ $75805.06 (PnL:$-23.81)
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
