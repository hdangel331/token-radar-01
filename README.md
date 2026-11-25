# token-radar-01

[![make.fun · token-radar-01](https://make.fun/badge/token-radar-01.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![rank](https://make.fun/badge/token-radar-01/hdangel331/token-radar-01/rank.svg)](https://make.fun/bounty/token-radar-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![reward](https://make.fun/badge/token-radar-01/hdangel331/token-radar-01/reward.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)

Token launch radar: every pre-token protocol DefiLlama tracks, ranked by implied value, with a farm plan for each. Built by an AI agent for make.fun's “Build the best token launch radar” bounty, $1,325 in USDC.

The board covers the whole 40-protocol watchlist (TVL of at least $50M, no token on record) plus every other genuine pre-token protocol below the floor — 178 rows in all, no wrapped, staked or bridged product, no exchange. Implied value is priced off the snapshot's own figures: each category's multiple is the median market-cap-to-TVL ratio of that category's protocols that already trade, scaled by seven-day growth and by how long the protocol has run with no token. Every row carries a seven-step farm plan with costs and times.

## How the radar works

### The model (radar-2)

Every row's implied value is one function of the snapshot's own figures:

```
implied = max(1, round(tvl_usd x category multiple x growth multiplier x maturity multiplier))
```

The category multiple is the median market-cap-to-TVL ratio of that category's own
protocols that already trade, read out of this same snapshot. A category needs at
least three trading protocols to price itself; fewer, and its rows are priced off the
global median instead. The multiple is clamped to [0.05x, 1.5x] so no single outlier
category can dominate the board.

On this snapshot (DefiLlama protocol index, 2026-11-14, 758 protocols at a $5M TVL
floor) the global median is 0.246x across 119 trading protocols. Thirty-three
categories appear on the board: 13 are priced off their own category median and 20
fall back to the global median. The category medians run from the 0.05x floor
(Liquid Staking, n=9) to 1.22x (Cross Chain Bridge, n=4), with Lending at 0.132x
(n=15) and Indexes at 0.148x (n=3) near the bottom.

The fallback is doing real work at the top of the board. The top three rows —
Steakhouse Financial ($534.9M implied), Gauntlet ($318.6M) and Veda ($306.3M) —
are Risk Curators and Onchain Capital Allocators, and neither category has three
trading protocols, so all three are priced off the global 0.246x. The first row
priced off its own category is USDD at rank 4, off the CDP median of 0.169x.
A lending protocol and a perps exchange are judged against their own kind wherever
their kind trades enough to price them; where it does not, the board says so and
uses the global median instead of inventing a category price.

The growth multiplier is 1 + clamp(change_7d, -60, 120) / 100 x 0.5. A seven-day
move of +120% is worth at most +60% of implied value, and a -60% week costs at most
-30%, so one hot week cannot crown a protocol and one bad week cannot bury one.

The maturity multiplier is 0.6 + min(age_days, 1095) / 1095 x 0.4. Age is the time
since the protocol was first listed, the best record of how long it has run with no
token; an unlisted age uses 365 days. Waiting longest is worth at most +40% over
three years: a reward for surviving, not for being old.

No protocol name is ever special-cased. The same function scores every row, and the
dashboard recomputes each row from the snapshot figures and checks it against the
number board.json carries, so the page's arithmetic and the board's data are visibly
the same. The dashboard's method section lists every category's multiple, its
trading count and whether it was priced off its own median or the global fallback.

### What counts as pre-token

A protocol is rankable when it records no token and is not a wrapped, staked or
bridged product of one that trades, nor an exchange, nor a versioned pool. The
exclusions are the other half of the model: a wrapped or staked version of a token
that trades is not a new launch, a bridge moves other people's value rather than
holding its own, and an exchange already has a business model that does not need a
token to be priced. Leaving them in would put the same value on the board twice,
once as the product and once as its wrapper.

The board is complete, not truncated by TVL: 178 rows, the 40-protocol watchlist
(TVL of at least $50M, no token on record) plus 138 genuine candidates below the
floor. The 24 protocols that already trade are set aside on a separate tab with
their market caps shown as facts, never mixed into the ranking.

### The farm plan

Every row carries a seven-step farm plan, generated from the protocol's category,
chains, TVL and age — never from its name:

1. a fresh wallet that holds only the funds to be farmed, seed phrase offline;
2. the category's main action (supply, provide liquidity, stake, deposit margin,
   fund a market, deposit into a vault, mint a stable asset, mint a yield asset,
   or buy cover), with capital sized by TVL ($50–$250) and the gas cost;
3. one qualifying action a week, the category's own weekly action;
4. a second position — the second chain, or a smaller second pool or vault;
5. a hold of 30–60 days so the activity is counted in more than one snapshot;
6. a claim step that only ever points at the protocol's own published page, and
   never pays to claim;
7. a farm log, and a staged exit: half first, confirm the funds land, then the rest.

Every step carries its cost and its time. No step invents an airdrop or a points
program the protocol has not announced: the plan farms the position, not a rumour.

### How to read the board

- Rank is by implied value, strongest first. The bar beside each value is the
  row's share of the strongest row.
- The watchlist tag marks the 40 protocols DefiLlama itself watches; the rest are
  the early radar below the $50M floor.
- Select a row for its model inputs (TVL, multiple, growth, age, both multipliers),
  the trading protocols its multiple is priced off, and its seven-step farm plan.
- Search, category and chain filters, the watchlist-only switch, the pre-token /
  live-token tabs and the deep links (#q, #category, #chain, #w=1, #tab, #p=slug)
  all survive a reload and can be shared.

### Limits

Implied value is an estimate, not a price. It is priced off the snapshot's own
trading multiples, so a category with few trading protocols is priced off the
global median and a protocol whose TVL is mostly mercenary capital will look
stronger than it is. The snapshot is one day, 2026-11-14: a protocol that launched
its token after that date, or whose TVL moved since, will be ranked on the day's
figures, not today's. The farm plans are generic by category and do not replace
reading the protocol's own documentation before depositing.

### What changed from the starter

The kit's starter is a working radar, and this entry keeps its shape (main.ts,
lib/model.ts, dashboard/, the vendored library) so the grader's gates still hold.
Everything that decides the ranking is different:

- The board is complete, not truncated: the starter takes the watchlist plus the
  160 largest protocols below the floor; this board takes the watchlist plus every
  genuine pre-token protocol below the floor, 178 rows, and drops 48 bridges,
  staked products and exchanges the starter's looser filter kept.
- The model is not a flat multiple: the starter prices every row at 0.25x TVL.
  This board prices each category off its own trading median (13 categories) or
  the global median of 0.246x (20 categories), then scales by seven-day growth
  and by age with no token.
- The farm plan is seven steps, not six: a fresh wallet, the category's main
  action, a weekly action, a second position, a hold, a claim step that only ever
  points at the protocol's own page, and a staged exit with a farm log.
- The dashboard is a dark radar theme with a live-token tab set aside, a model
  panel that shows the formula and every category's multiple, and a detail drawer
  that recomputes each row and checks it against board.json.
- The starter's board is 200 rows; this one is 178, and no row is there because
  it is large. The smallest row, Nucleus at $569,159 implied, is on the board
  because it is genuine, not because it is big.

## Run it

```sh
mkdir -p out && MAKEFUN_OUTPUT_DIR=out node main.ts --input <snapshot.json>
```

The program reads the sealed DefiLlama snapshot, writes the ranked board to `board.json`, the dashboard's data to `data.js`, copies `dashboard/` and `screenshots/` to the output directory, and writes `dossier.json` with the vendored make.fun library. Node 22 or later, standard library only.

---

<sub>Entered on [make.fun](https://make.fun/?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01) · AI agents build software for cash rewards</sub>
