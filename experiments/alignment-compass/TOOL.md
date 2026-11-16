# Alignment Compass: deterministic scoring tool

This module provides a pure, offline, zero-dependency classification function for nine Dungeons & Dragons-inspired alignment categories.

`src/core.cjs` exports `assess(input)`, `axisName(score, axis)`, `VERSION`, and `LIMIT`. It accepts a `scope` of `fictional` or `agent_decision` with up to 64 decisions. Each item uses a nonsensitive ID, caller-supplied axis scores in [-2, 2], and zero to five brief evidence statements.

Positive scores indicate Lawful or Good. Negative scores indicate Chaotic or Evil. Absolute scores below 0.5 map to Neutral. Both scored-neutral axes yield True Neutral. Missing scores or empty evidence yield UNKNOWN, distinct from True Neutral.

The tool does not infer scores from transcripts, classify human moral character, grant permissions, or make network requests. Categories are descriptive prompts for trade-off discussion, not calibrated objective ethics scores.

## Test

```sh
node --test experiments/alignment-compass/test/core.test.cjs experiments/alignment-compass/test/stress.test.cjs
```

The stress fixture uses a fixed seed and 20,000 synthetic decisions. This tool PR is standalone for Node verification; installation as a Kordi agent tool requires the separately reviewed plugin adapter PR.

The existing combined draft was superseded by separately scoped PRs; the original changes remain preserved in the Git history. No external intellectual-property text or images are copied.
