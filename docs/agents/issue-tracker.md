# Issue tracker: Linear

Issues for this repo live in Linear, in the **`Agents`** project on the **`Engineering`** team. Identifiers look like `ENG-123`. One board covers `Kale-BI/skillset`, `Kale-BI/kaley-agent`, `Kale-BI/symphony`, `Kale-BI/dossiers` and this repo, so name the repo in the issue.

## Conventions

- Use the Linear MCP tools (`get_issue`, `list_issues`, `save_issue`, `save_comment`, `list_issue_labels`, `list_issue_statuses`).
- **Create an issue**: `save_issue` with the team and this project set.
- **Read**: `get_issue` by identifier; `list_comments` for the thread.
- **List**: `list_issues` filtered by this project, and by state or label as needed.
- **Comment**: `save_comment` on the issue.
- **Labels**: `save_issue` with the updated label set. Labels are team-scoped.
- **Close**: `save_issue` setting the state to Done (actioned) or Canceled (will not fix).
- **State names**: list the team's issue statuses before setting one. A wrong state name makes a save silently no-op.
- **Sub-issues and blocking**: set `parentId` at creation for tickets cut from a spec; use Linear's native `blockedBy` relation for gates.
- **Provenance**: an autonomous agent creates issues and comments through its **own** Linear identity, never through a human's credentials. The creator field is how the team tells agent-filed work from human-filed work. Interactive sessions where a human is driving file as that human.

## When a skill says "publish to the issue tracker"

Create a Linear issue in this project.

## When a skill says "fetch the relevant ticket"

Fetch the Linear issue by identifier, including comments.

**Images in tickets are readable; check the URL for `?signature=` before choosing how.** The Linear MCP returns bodies with *signed* `uploads.linear.app` URLs, public and valid for a few minutes: use `extract_images`, or fetch each one the moment you read the text. Raw GraphQL returns the stored markdown with *unsigned* URLs, which answer 401 to a plain GET and need a Linear token in the `Authorization` header. With no token and no way to fetch, say so in the plan and ask the maintainer to transcribe the images into a comment; never silently skip them.

## How to write here

Everything on this tracker is read by agents and by colleagues who were not in the conversation that produced it. The tracker holds the **summary**: what a colleague would write, short and on point. The full material lives in the effort's **artefact** (see `docs/agents/artefact.md`), linked from the issue.

- Open with the verdict, in one sentence the reader could stop at.
- Refer to issues, specs and pull requests by title, with the identifier inside the link.
- Titles state the problem, not the intended fix.
- Keep what is checkable (a number, a test name, a linked ticket); cut the adjectives.
- Whole sentences, glossary terms, no shorthand coined mid-session.
- Write the body of issues you create. On an issue someone else wrote, comment; never rewrite their body.
- A comment says what happened and what the reader owes you, with a link to the artefact for everything that needs reading rather than a quick look. Decision cards and grilling agendas are the exception: they stay inline, written to be answered from a phone.
- Mentions use the Linear display name (`@davide`), not full names.

## What a ready ticket contains

A ticket an agent can start unattended states, in its body or its artefact section:

- **What to build**: the end-to-end behaviour it makes work, from the user's side.
- **Evidence**: the proof a human will look at to say "it works", one of: screenshot or GIF, terminal transcript, query and rows, test run, live URL. A passing test suite is not evidence.
- **Plan**: the areas touched and the seams the work is tested at, plus every fact the work depends on that was looked up rather than assumed: how to run it, what already exists, what was verified and how.
- **Blocked by**: the tickets that gate it, set as Linear's native `blockedBy` relation, or none.

Tickets cut from a spec are the spec's sub-issues (`parentId` set to the spec). Evidence and human review happen on the ticket a human opened: the spec when tickets were cut from one, otherwise the ticket itself; sub-issues go straight to Done.

A question only the maintainer can answer is a **blank**. Name it, with its options and a recommendation, instead of guessing.

## The follow-up ticket

An out-of-scope finding filed from other work is a ticket of its own, created untriaged; a human decides when it enters the queue. The title states the problem, never the intended fix. A finding that cannot fill the first two fields from observed facts is an opinion, not a ticket.

```markdown
**Found while:** <parent ticket, linked, and what happened there that surfaced this>
**Cost of not doing it:** <who hits what, when>
**Current behaviour:** <readable by someone who never saw the parent work>
**Desired behaviour:** <the behaviour wanted, not the change to make>
```

## Where a decision record lands

A grilling session's decision record is an issue: the summary in the body, the full record in the artefact. When an issue prompted the session, the record goes on that issue instead of a new one.

## Triage state mapping

The triage skill's four routes render here as **states**, not labels. State names below were confirmed against `list_issue_statuses` on the Engineering team on 2026-09-10.

| Route | Here |
|---|---|
| *(untriaged)* | state **Backlog** |
| `ready-for-agent` | state **Todo**, delegation kept |
| `ready-for-human` | state **Todo**, delegation removed, creator tagged |
| `needs-decision` | state **Needs Decision** (+ decision card comment) |
| `needs-grilling` | state **Needs Grilling** (+ agenda comment) |
| *duplicate* | state **Duplicate**, `duplicateOf` set to the survivor |
| *declined* | state **Not Planned**, reason in the closing comment |
| *already done or invalid* | state **Done** or **Canceled**, evidence in the closing comment |

**Not Planned is the record of what the project chose not to do.** One ticket per declined ask, with the reason in its closing comment; later tickets asking the same thing are marked Duplicate of it, so one ticket accretes the ask's history. Canceled and Done stay for tickets that were invalid or already done, so a search can tell a deliberate no from a never-was.

**Untriaged work waits in Backlog.** A delegated Backlog ticket gets a triage session, and the session always exits Backlog into one of the states above. Needs Decision and Needs Grilling are outside the dispatcher's active states, so a parked ticket cannot re-dispatch.

**Dispatch:** manual; runs start by hand with `/triage`. The Agents Symphony process does not clone this repo, so an issue for this repo delegated to Symphony lands in a workspace that cannot edit it: leave it undelegated until the process's clone list names `Kale-BI/software-factory`. Where Symphony runs, the lever is *delegating* the issue to the **Symphony** agent, with the state naming the verb (**Backlog** = triage, **Todo** = implement, **Ship** = close out). Delegating in Backlog is consent for the whole ride. Undelegated tickets are untouched in every state; take work back by removing the delegation.

**Resuming a parked ticket is one drag.** Answer the decision card or hold the grilling session, then move the ticket to **Backlog** (re-triage reads the reply) or straight to **Todo** (the answer made it ready).

Pipeline states beyond triage: **In Progress** = agent working; **Human Review** = a human gate (spec sign-off, or PR review once a PR is linked); **Ship** = landing in progress, never parked; **Blocked** = an implementation that cannot proceed, always carrying the agent's diagnosis, never a triage destination; **Done** = verified in production (or merged, for repos with no deploy).

## Wayfinding operations

Used by `/wayfinder`. The **map** is a single Linear issue with **child** tickets as sub-issues.

- **Map**: an issue in this project labeled `wayfinder:map`, holding the Notes / Decisions-so-far / Fog body.
- **Child ticket**: a sub-issue of the map (set `parentId`), labeled `wayfinder:<type>` (`research`, `prototype`, `grilling`, `task`).
- **Blocking**: Linear's native blocked-by relations. A ticket is unblocked when every blocker is done.
- **Frontier**: open, unblocked, unassigned children of the map; first in map order wins.
- **Claim**: assign the issue to yourself before any work.
- **Resolve**: post the answer as a comment, mark the issue Done, append a context pointer (gist + link) to the map's Decisions-so-far.
