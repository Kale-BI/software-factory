# The Software Factory

The public site describing how Kale runs engineering with a small team and a large number of
agent sessions — the process, the board, the skill tree, and the actual prompts, published in
full. Live at [factory.eatkale.ai](https://factory.eatkale.ai).

Plain HTML, no build step: edit a page, push to `main`, GitHub Pages serves it.

## Where the content comes from

The site renders the team's process records, which live with the code they govern:

| Page | Source of truth |
|---|---|
| The machine, foundations, actors | the engineering-process decision record in the skillset repo |
| The morning triage | the `triage` skill and the morning report's script |
| Symphony | the dispatcher's `prompt.md`, its decision records, and the `symphony-run` skill |
| The board | the board configuration and the tracker instructions' state mapping |
| The skill tree | the skill-tree decision records and the skillset's own skill tables |
| Evidence and dossiers | the tracker and artefact instructions the setup skill seeds into each repo |

When a source changes, the page rendering it must change in the same effort. The internal
map for this obligation lives in the skillset repo's `docs/factory-map.md`.

## Redaction

This repo is public and Pages publishes its full history. Nothing account-shaped is ever
committed: no credentials, no internal hostnames, no channel or account identifiers. Prompts
are published verbatim except for those removals.
