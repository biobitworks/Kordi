# Alignment Compass: Kordi plugin adapter

This package manifest and JS/CJS extension registers a Kordi-native `alignment_compass` tool and `/alignment` command. It is part of a four-PR review split from original PR #2596.

## Dependencies and activation

**Requires the separate scoring-tool PR (#2598 issue), which installs `src/core.cjs`.** Until that PR merges, the plugin adapter cannot load independently; do not activate the extension in any Kordi environment.

Kordi's extension loader supports local `.cjs` extensions, and the package manifest advertises directories for `extensions`, `skills`, and `prompts`. The separately reviewed skill and agent PRs supply the latter resources; missing optional directories are not treated as installed.

Once the tool PR and plugin PR are merged, test with a source-built Kordi CLI in a disposable offline workspace:

```sh
kordi install --local ./experiments/alignment-compass
kordi list --local
```

Alternatively, ad-hoc extension loading is supported:

```sh
kordi -e ./experiments/alignment-compass/extensions/alignment-compass.cjs
```

**Never use a signed-in product Kordi desktop session or shared production backend for this test.** The Kordi app's runtime is separate from a source checkout and cannot be assumed to have loaded the package.

## Tool interface

```json
{
  "scope": "agent_decision",
  "items": [
    {
      "id": "approval-before-write",
      "lawChaos": 2,
      "goodEvil": 1,
      "evidence": ["Explicit authorization was requested before writing."]
    },
    { "id": "insufficient-input" }
  ]
}
```

The first item is Lawful Good. The second is UNKNOWN. A missing score or evidence is permitted by the Kordi registration schema to support abstention.

`/alignment help` prints usage; `/alignment <JSON>` classifies a supplied payload. The extension makes no external HTTP requests and does not change tool permissions or provider authentication.

## Testing and limitations

Run the integration tests only in a combined checkout containing both the scoring-tool and plugin changes:

```sh
node --test experiments/alignment-compass/test/host.test.cjs
```

The integration test spawns the real `agent/crates/plugin-host/js/host.js` process and covers registration, tool and command calls, invalid input, UNKNOWN abstention, and 150 concurrent synthetic requests.

This PR alone is not an end-to-end product integration. The additional skill and agent reviewer resources are separate PRs. Complete Kordi desktop integration, cloud agents, and a ChatGPT Kordi connector are not claimed.

## Attribution

Inspired by [D&D alignment](https://en.wikipedia.org/wiki/Alignment_(Dungeons_%26_Dragons)) and [EasyDamus examples](https://easydamus.com/alignment.html), without copying their prose or images. Not affiliated with the D&D publisher. The MIT terms of the Kordi contribution apply.
