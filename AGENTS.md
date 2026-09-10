# Software Factory — agent instructions

The public site describing how Kale runs engineering; `README.md` says what each page renders and where its source of truth lives. Plain HTML, no build step.

## Agent skills

### Issue tracker

Linear, project **Agents** on the **Engineering** team, identifiers `ENG-123`; one board covers skillset, kaley-agent, symphony, dossiers and this repo, so name the repo in the issue. See `docs/agents/issue-tracker.md`.

### Artefact

The effort's page on the dossiers site (`Kale-BI/dossiers`, served at dossiers.eatkale.ai). See `docs/agents/artefact.md`.

### Domain docs

Single-context: `CONTEXT.md` and `docs/adr/` at the repo root, created lazily. See `docs/agents/domain.md`.

### Delivery

Read by `ship`.

- **Base branch:** `main`
- **Merge:** squash; remote branch left
- **Deploy:** none, the merge is the release: GitHub Pages serves `main` at factory.eatkale.ai with no workflow to run
- **Deploy green:** `gh api repos/Kale-BI/software-factory/pages/builds/latest --jq '.status + " " + .created_at'` reports `built` with a time after the merge
- **Canary:** open the changed page at `https://factory.eatkale.ai/<page>.html` and see the change rendered; then check that the source the page restates (the `README.md` table names it) still says what the page now says
- **Local cleanup:** remove the ticket worktree, delete the local branch, fast-forward `main`

## Guardrails

- **This repo is public and Pages publishes its full history.** Nothing account-shaped is ever committed: no credentials, no internal hostnames, no channel or account identifiers. A prompt is published verbatim except for those removals.
- **A page is a restatement, never a source.** When a page and its source disagree, the source wins and the page changes; a fact that exists only on a page belongs in its source first. The pairings live in `README.md` here and in `docs/factory-map.md` of the skillset repo.
