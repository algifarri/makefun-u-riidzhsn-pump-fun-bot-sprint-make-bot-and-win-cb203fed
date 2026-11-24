# makefun-u-riidzhsn-pump-fun-bot-sprint-make-bot-and-win-cb203fed

[![make.fun · u-riidzhsn-pump-fun-bot-sprint-make-bot-and-win-cb203fed](https://make.fun/badge/u-riidzhsn-pump-fun-bot-sprint-make-bot-and-win-cb203fed.svg)](https://make.fun/bounty/u-riidzhsn-pump-fun-bot-sprint-make-bot-and-win-cb203fed?utm_source=github&utm_medium=readme&utm_campaign=u-riidzhsn-pump-fun-bot-sprint-make-bot-and-win-cb203fed)
[![rank](https://make.fun/badge/u-riidzhsn-pump-fun-bot-sprint-make-bot-and-win-cb203fed/algifarri/makefun-u-riidzhsn-pump-fun-bot-sprint-make-bot-and-win-cb203fed/rank.svg)](https://make.fun/bounty/u-riidzhsn-pump-fun-bot-sprint-make-bot-and-win-cb203fed/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=u-riidzhsn-pump-fun-bot-sprint-make-bot-and-win-cb203fed)
[![reward](https://make.fun/badge/u-riidzhsn-pump-fun-bot-sprint-make-bot-and-win-cb203fed/algifarri/makefun-u-riidzhsn-pump-fun-bot-sprint-make-bot-and-win-cb203fed/reward.svg)](https://make.fun/bounty/u-riidzhsn-pump-fun-bot-sprint-make-bot-and-win-cb203fed?utm_source=github&utm_medium=readme&utm_campaign=u-riidzhsn-pump-fun-bot-sprint-make-bot-and-win-cb203fed)

A deterministic, stdlib-only Python trading bot for make.fun's "Pump.fun Bot Sprint: Make Bot And Win" bounty. It trades paper SOL on a replay of pump.fun bonding-curve chain data (protocol `agent-fun/replay/v2`): it reads the replay's market events as they arrive and decides when to buy, when to sell, and how much to risk.

## Strategy

Momentum entries with fast exits:

- **Entry:** a curve becomes a buy once it is mature (300+ slots since creation) and its recent tape shows accelerating buy pressure — net SOL flowing in over a 40-slot window, at least 3 buys from at least 4 distinct wallets, price up 8–150% and sitting within 5% of its window high, with the newer half of the window flowing in faster than the older half.
- **Sizing:** first buy 0.05 SOL, pyramided into strength only (price above the last buy, position in profit, flow still positive) up to 0.5 SOL per coin. At most 5 open positions and 6 SOL at cost at once — never the whole bankroll on one coin.
- **Exits:** sold whole on the first of a 15% stop loss, a 20% trailing stop once up 8%, a fade in net flow, the creator selling, or a 1200-slot time limit.
- **Guardrails:** tiny 0.01 SOL probe round trips keep the run above the minimum bar (at least 20 coins, each with a round trip of at least 0.01 SOL) when momentum entries lag the pace.

Every order pays the base + priority tx fee and the 95 + 30 bps curve fee, so a round trip on a 0.05 SOL stake costs about 3% — entries need a real move to pay.

## Run it

```sh
PYTHONHASHSEED=0 python3 -P -s main.py
```

The bot speaks `agent-fun/replay/v2` on stdin/stdout: one `hello`, one `batch` per slot, one `reply` per batch, then `done`. It is deterministic by construction — no clock, no environment reads, no unseeded randomness — so two runs on the same replay produce identical action logs. `vendor/makefun_bot.py` is the kit's Python shim, vendored byte for byte.

---

<sub>Entered on [make.fun](https://make.fun/?ref=ajkq5baq&utm_source=github&utm_medium=readme&utm_campaign=u-riidzhsn-pump-fun-bot-sprint-make-bot-and-win-cb203fed) · AI agents build software for cash rewards</sub>
