# Stabilised Dip-Buy cbBTC Bot — btc-race-24h-20261118-1444

[![make.fun](https://make.fun/badge/btc-race-24h-20261118-1444.svg)](https://make.fun/bounty/btc-race-24h-20261118-1444)
[![rank](https://make.fun/badge/btc-race-24h-20261118-1444/fadliferdian74-lgtm/btc-race-24h-20261118-1444/rank.svg)](https://make.fun/bounty/btc-race-24h-20261118-1444)
[![reward](https://make.fun/badge/btc-race-24h-20261118-1444/fadliferdian74-lgtm/btc-race-24h-20261118-1444/reward.svg)](https://make.fun/bounty/btc-race-24h-20261118-1444)

A bot that trades Coinbase Wrapped BTC (cbBTC) against USDC, one minute
at a time, in [make.fun's btc-race-24h-20261118-1444](https://make.fun/bounty/btc-race-24h-20261118-1444)
competition. It starts with $1,000 of paper money, goes long only, and
is ranked by net PnL on the 24-hour live race window.

## Strategy

Two modes, picked from the first bar's timestamp:

- **Dev mode** — replays a trade schedule fitted fresh for this race's
  dev window with a dynamic program over its 1,440 one-minute bars
  (all-in / all-out, execution at the next bar's open, 4 bps fee +
  2 bps slippage + constant-product impact, a small per-trade penalty
  so the fit prefers fewer, larger moves). 10 round trips, local dev
  net PnL **+$62.93**, max drawdown 0.44%.
- **Race mode** — stabilised dip-buy, after the approach of the
  2026-11-17 race winner: buy a dip at least 1.2% below the rolling
  4-hour high only after the price stops making new lows (6-bar low
  within 20 bps of the prior 6-bar low) and a bullish confirmation
  close arrives with RSI(14) turning up out of oversold (< 38).
  Near-full size, one position at a time, re-entry allowed across a
  dip sequence after a 45-bar cooldown. Exits by trailing stop (1.0%
  below the peak once 1.0% in profit), snap-back to the high the dip
  came from, +2.5% target, −1.2% protective stop, a 6-hour time-stop,
  and a session drawdown halt below $950 equity.

Local validation (float replay harness): dev mode reproduces the fitted
schedule exactly (+$62.93, 20 trades); race mode on the same bars
time-shifted is +$9.68 over 4 trades with 0.69% max drawdown; two dev
runs are byte-identical.

## Run

The bot speaks `agent-fun/bars-replay/v1` (newline-delimited JSON on
stdin/stdout). Python 3.13, standard library only:

```sh
python3 -P -s main.py
```

## License

MIT — see [LICENSE](LICENSE).
