# PoE2 leech calculation

Integrated upstream PR #2591 (e5afe148da7683b07f744eb39488134d90f4caf5),
which fixes issue #328. Source changes retain their original authors/history.

- One active recovery instance per resource, baseline one-second recovery.
- Cap the total hit used for leech at 40,000 and preserve damage-type ratios.
- Apply the level-indexed monster leech resistance exported from MonsterLevelStats.
- Remove PoE1 pool-based instance and total-rate caps.
- Increased amount leeched scales the amount; faster leech changes duration.

GGG source: https://www.pathofexile.com/forum/view-thread/3932540 (Player Changes).

The strongest overlapping instance uses an estimated uniform damage-roll/critical
selection model. These are engine predictions, not measurements in the game.
A sustained single-skill output must not be added across different skills as if
all their leech instances recovered concurrently. Low-frequency rotation estimates
must account for hit intervals, the one-second duration, and competing instances.

Additional regression tests cover mana 1% -> 3% scaling, independence from maximum
mana, and level-80 enemy resistance. Stonefist tests remain enabled.
