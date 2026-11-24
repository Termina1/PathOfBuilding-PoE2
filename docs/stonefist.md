# Way of the Stonefist preview

Port of upstream PR #2350 by FlowaBoi / BligenN, adapted to the current dev branch.

Allocating Way of the Stonefist transforms equipped gloves in the calculation
pipeline. The original item and its exported text are not replaced, so removing
the passive restores the original item. Tooltips preview the transformed base,
per-level defence and modifiers. Rune and enchant modifiers remain unchanged.
Already imported Fists of Stone are not transformed a second time.

## Accuracy boundary

Original glove text does not necessarily identify an exact converted roll. For
example, a fixed +2 melee-level affix maps to a ranged skill-quality modifier.
This preview resolves target ranges at their tier midpoints and explicitly warns
in the tooltip when that happens. Fractional critical chance is retained. Do not
interpret midpoint estimates as validated exact in-game rolls: import the game's
transformed Fists of Stone for an exact comparison. This PR does not claim to
reverse-engineer the server's modifier RNG.

## Tests

Run the dedicated regression file with the project's test container:

```sh
docker run --rm --platform linux/amd64 -e HOME=/tmp \
  -v "$PWD:/workdir" -w /workdir \
  ghcr.io/pathofbuildingcommunity/pathofbuilding-tests:latest \
  busted --lua=luajit ../spec/System/TestStonefist_spec.lua
```

The tests cover base and affix conversion, ordinary and Runeforged implicits,
unscalable annotations, fractional critical chance, cache invalidation, level
changes, source immutability, non-glove guards, already-transformed imports,
enchant preservation and activating/deactivating the actual ascendancy node.

Generated mappings and target data originate from the upstream PR. The exporter
changes are included so these tables can be regenerated from game data; the full
GGPK export was not rerun as part of this port.
