# Autotrader-FutureFocus

This repository contains a Python notebook that implements a multi-strategy automated trading bot on the Optibook platform, built in Optiver's FutureFocus program. The autotrader is built for a simulated trading environment containing only two higly correlated stocks - PHILIPS_A and PHILIPS_B. It connects to the exchange, retrieves live market data, and executes trades using market making, pair trading arbitrage, and lead-lag strategies.

## Features

- Connects to an exchange server (Optibook)
- Retrieves live order books, trades, and positions  
- Executes automated trades based on multiple strategies  
- Monitors and manages inventory & PnL
  
The autotrader primarily uses the following strategies:

### (i) Market Making
- Instead of quoting around a mid-price, the autotrader places bids and offers on PHILIPS_A and PHILIPS_B to align their order books, since they represent the same stock on different exchanges.
- If PHILIPS_A’s best bid > PHILIPS_B’s best bid, it places a bid on PHILIPS_B at PHILIPS_A's best bid (and vice versa).
- If PHILIPS_A’s best ask < PHILIPS_B’s best ask, it places an offer on PHILIPS_B at PHILIPS_A’s best ask (and vice versa).
- This effectively tightens the spread across exchanges and captures price discrepancies.

### (ii) Pair Trading Arbitrage
- The autotrader monitors the price relationship between the two correlated stocks and places offsetting positions when the best bid of one is higher than the best offer of the other.
- It buys the undervalued stock and sells the overvalued one, aiming to profit as prices revert to their historical mean.
- It then squares off the position when the two prices converge to lock in profits, and may resort to aggressive rebalancing if it becomes too long or short on either stock.

### (iii) Lead-Lag Strategy
- The autotrader identifies a leader stock that typically moves ahead of a lagger, using short-term price moves in the leader to predict moves in the lagger. In this case, PHILIPS_A is observed to act as the leader and PHILIPS_B as the lagger most of the time.
- It places trades on the lagger based on signals from the leader, capturing small inefficiencies in price adjustments.

Across all these strategies, the autotrader adjusts the order size (volume) depending on the current spread:
- Tight spreads → smaller volume: to avoid excessive trading costs.
- Wide spreads → larger volume: to capture bigger opportunities.

This makes the strategies more responsive to prevailing market conditions.


## Quick Start

```python
import time
from optibook.synchronous_client import Exchange

exchange = Exchange()
exchange.connect()

