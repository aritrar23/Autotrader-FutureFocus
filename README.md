# Autotrader-FutureFocus

This repository contains a Python notebook that implements a multi-strategy automated trading bot on the Optibook platform, built in Optiver's FutureFocus program. The autotrader is built for a simulated trading environment containing only two higly correlated stocks - PHILIPS_A and PHILIPS_B. It connects to the exchange, retrieves live market data, and executes trades using market making, pair trading arbitrage, and lead-lag strategies.

## Features

- Connects to an exchange server (Optibook)
- Retrieves live order books, trades, and positions  
- Executes automated trades based on multiple strategies  
- Dynamically adjusts order volumes as a function of market spread  
- Monitors and manages inventory & PnL

### Market Making
- Continuously quotes buy and sell orders around the mid-price.
- Targets capturing the bid-ask spread.
- **Adaptive volume:** order size increases with the spread, aiming to capitalize more when spreads are wide.

### Pair Trading Arbitrage
- Monitors price relationships between two correlated instruments (e.g. futures vs ETF, or similar underlyings).
- Places offsetting trades to exploit deviations from expected price ratios.
- Positions are managed to converge back to the mean.

### Lead-Lag Strategy
- Identifies instruments where one typically moves slightly ahead of another (the *leader* and *lagger*).
- Uses movements in the leader to predict short-term movements in the lagger, placing trades to capture this microstructure inefficiency.


## Dynamic Volume as a Function of Spread
Across these strategies, the bot adjusts the order size (volume) depending on the current spread:
- **Tight spreads → smaller volume:** protects against excessive trading costs.
- **Wide spreads → larger volume:** captures bigger opportunities.

This makes the strategies more responsive to market conditions.


## Quick Start

```python
import time
from optibook.synchronous_client import Exchange

exchange = Exchange()
exchange.connect()

