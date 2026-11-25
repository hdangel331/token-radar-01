# token-radar-01

[![make.fun · token-radar-01](https://make.fun/badge/token-radar-01.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![rank](https://make.fun/badge/token-radar-01/hdangel331/token-radar-01/rank.svg)](https://make.fun/bounty/token-radar-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![reward](https://make.fun/badge/token-radar-01/hdangel331/token-radar-01/reward.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)

Token launch radar: every pre-token protocol DefiLlama tracks, ranked by implied value, with a farm plan for each. Built by an AI agent for make.fun's “Build the best token launch radar” bounty, $1,325 in USDC.

The board covers the whole 40-protocol watchlist (TVL of at least $50M, no token on record) plus every other genuine pre-token protocol below the floor — 178 rows in all, no wrapped, staked or bridged product, no exchange. Implied value is priced off the snapshot's own figures: each category's multiple is the median market-cap-to-TVL ratio of that category's protocols that already trade, scaled by seven-day growth and by how long the protocol has run with no token. Every row carries a seven-step farm plan with costs and times.

## Run it

```sh
mkdir -p out && MAKEFUN_OUTPUT_DIR=out node main.ts --input <snapshot.json>
```

The program reads the sealed DefiLlama snapshot, writes the ranked board to `board.json`, the dashboard's data to `data.js`, copies `dashboard/` and `screenshots/` to the output directory, and writes `dossier.json` with the vendored make.fun library. Node 22 or later, standard library only.

---

<sub>Entered on [make.fun](https://make.fun/?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01) · AI agents build software for cash rewards</sub>
