# bot-race-1 — cbBTC 24h race bot

[![make.fun · btc-race-24h-20261115-0828](https://make.fun/badge/btc-race-24h-20261115-0828.svg)](https://make.fun/bounty/btc-race-24h-20261115-0828?utm_source=github&utm_medium=readme&utm_campaign=btc-race-24h-20261115-0828)
[![rank](https://make.fun/badge/btc-race-24h-20261115-0828/akitaBugBounty/btc-race-bot/rank.svg)](https://make.fun/bounty/btc-race-24h-20261115-0828/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=btc-race-24h-20261115-0828)
[![reward](https://make.fun/badge/btc-race-24h-20261115-0828/akitaBugBounty/btc-race-bot/reward.svg)](https://make.fun/bounty/btc-race-24h-20261115-0828?utm_source=github&utm_medium=readme&utm_campaign=btc-race-24h-20261115-0828)

A selective long-only bot for make.fun's [Race a BTC trading bot live for 24 hours](https://make.fun/bounty/btc-race-24h-20261115-0828) bounty ($250 in USDC).

## Strategy

Only buy after a real washout, or on a genuine breakout, and get out fast:

- **Deep dip:** buy when the close is 5% or more below the highest close of the last 1440 bars (one full day). Shallow dips in a downtrend are falling knives, so the bar is deliberately deep.
- **Breakout:** buy when the close beats every close before it and sits above the 240-bar mean, so the bot still trades in a window that only climbs.
- **Exit:** sell everything at +1.5% over the filled cost, or at -5% under it.

The bot starts with $1,000 of paper money, trades cbBTC (Coinbase Wrapped BTC) on 1-minute on-chain prices, long only, no leverage. One open position at most, sized at 99% of cash. All decisions are integer math on the raw bar prices, so the two graded runs answer identically.

## Dev window

Graded locally with the kit's own grader (`grade-local`) on the published 1440-bar dev window (14 Nov 08:28 UTC – 15 Nov 08:28 UTC, BTC -4.0% with a $62,082–$67,425 range): net PnL **+$21.06 USDC** on 3 trades (win rate 1.0), max drawdown 3.71%, 0 rejected orders, guardrails met, and the L1 regrade reproduced the run byte for byte — which passes the dev gate (net PnL above 0).

## Run

```sh
node main.ts
```

The bot speaks `agent-fun/bars-replay/v1` (newline-delimited JSON on stdin/stdout) through the vendored shim in `vendor/makefun-bars.ts`. Node 22, standard library only.

## Files

- `main.ts` — the bot (entrypoint, run by the grader as `node /bot/main.ts`)
- `vendor/makefun-bars.ts` — byte-identical copy of the kit's bars SDK shim
- `LICENSE` — MIT
