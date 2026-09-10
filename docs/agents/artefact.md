# Artefact: published HTML site

An **artefact** is the long-form record of one effort: the grilling record, the spec, the plan, the evidence, accreted stage by stage on one page. Agents and humans read the same page; the tracker links it and keeps the summary. Here artefacts live in a separate repo served as a static site.

## Where

- **Repo:** `Kale-BI/dossiers`, served at `https://dossiers.eatkale.ai`, deployed by push to `main` via Workers Builds.
- **Page:** `public/<effort-slug>.html`, served at `https://dossiers.eatkale.ai/<effort-slug>`, named after the **effort**, never after a ticket. One effort, one page. Never rename a slug once it has been linked from the tracker; a renamed effort keeps the old file as a one-line redirect.
- **Index:** `public/index.html`, hand-maintained, newest first. Add the line when the page is created.

A skill creates the page the first time it has something long-form to write, never ahead of need, starting from `templates/dossier.html`.

## Shape

- The top of the page is the **current contract**: what to build, the evidence, the plan, in the ready-ticket shape from `docs/agents/issue-tracker.md`. Later stages rewrite it in place.
- Below it, one section per stage, each with a stable anchor (`#grilling`, `#spec`, `#plan`, `#evidence`) so a ticket can link the part that concerns it. A stage already written is a record of what was believed at the time: later stages go after it, corrections are dated entries, never edits to history.
- Every evidence entry carries a date and a link to the pull request, run, or file that backs it.
- If a paragraph runs past six lines it wants to be a table, a list or a callout. The bar is a colleague skimming for the one decision they need.
- All styling lives in the site's shared stylesheet, `public/assets/dossier.css`; pages declare no colours, fonts or spacing of their own. A shape the stylesheet lacks is added to the stylesheet, not to the page.

## Linking

Attach the artefact to the issue with `create_attachment` so it renders as a card, titled with the effort name, with the stage anchor when only one stage is relevant. Child tickets of an effort link anchors on the effort's page rather than opening pages of their own.

## Landing

Pages land on the site's main branch directly and deploy on push. A document describing work in flight is useless behind a review queue.
