---
title: Escalations — protocol and index
type: index
part_of: "[[Setup]]"
last_verified: 2026-08-10
changelog:
  - { date: 2026-08-10, agent: eu-uk-regdev-paralegal, note: "Initial filing — escalations moved to root Escalations/, one file per escalation, callout protocol" }
uuid: 7b1e9c2a-4d5f-4a6b-9c8d-1e2f3a4b5c6d
publish: true
---

# Escalations

This folder is the collaboration surface between the humans and agents working
on this vault. Each escalation is **one file**. Open escalations live here;
resolved ones move to `Resolved/`. You work them — and any note in the vault —
using **callouts**.

## The callout protocol

Leave a callout in any note to start or continue a thread. Five types:

| Callout | Meaning | Creates work? |
|---|---|---|
| `[!todo]` | please do this | yes — for whoever it is addressed to |
| `[!question]` | please answer this | yes |
| `[!success]` | done — here is what happened | no (a report) |
| `[!failure]` | tried, could not | no (but stays visible) |
| `[!warning]` | caution / FYI | no |

Only `[!todo]` and `[!question]` create work, and only for the *other* party: a
human's todo is agent work; an agent's question is human work.

**Addressing.** Open the title with `@handle` to address someone
(`> [!todo] @reginald verify this deadline`). Reginald's handle is `reginald`;
unaddressed todos and questions default to it.

**Replies nest.** Answer a callout by nesting one blockquote level under it, so
the exchange reads as a thread. The nesting *is* how completion is tracked — an
item is only done when a nested reply exists.

```markdown
> [!todo] @reginald verify the CP26/21 deadline
> The Index.md row says 14 Aug — check the FCA page directly.
>
> > [!success] 2026-08-11 — Verified, no change needed
> > Fetched fca.org.uk/publications/consultation-papers/cp26-21 directly.
> > Deadline confirmed as 14 August 2026.
```

Conventions:

- Reply **nested under** the callout you are answering, never as a new
  top-level callout.
- Agent reply titles start with the ISO date, then a short outcome.
- A `[!failure]` says what was tried and what is needed to proceed.
- Never edit or delete another author's callout text. To withdraw your own
  request, delete the callout — its work item is then marked `not-actioned`.
- A thread can go several turns: an agent `[!question]` back to a human is
  itself nested, and the human's next callout re-enters the queue.

Callouts are ignored inside `Archive/`, `Setup/Logs/`, `.obsidian/`, and
fenced code blocks (so this document's examples stay inert).

## Escalation files

Filename: `YYYY-MM-DD - Short slug.md` (the raised date). **Do not put the
number in the filename** — Reginald assigns `number:` in frontmatter, which
avoids collisions when two people raise escalations at once.

Frontmatter:

```yaml
---
title: Short human title
type: escalation
number: 38                       # assigned by Reginald's sync — omit when raising
status: pending                  # pending | actioned-by-agent | actioned-by-human | ignored
raised_date: 2026-08-10
status_changed_date: 2026-08-10
raised_by: research-new-developments   # a task name, or your git author name
priority: medium                 # optional
topic: Payments and E-money      # optional
uuid: <freshly generated UUIDv4>
---
```

Body: the issue, sources, and what is recommended — then the callout thread.

## Status and where the file lives

The rule, enforced automatically in both directions:

- `status: pending` ⟺ the file lives in `Escalations/`
- any other status ⟺ the file lives in `Escalations/Resolved/`

So you can resolve an escalation **either** by setting its `status`
(Reginald moves the file) **or** by moving the file to `Resolved/` (Reginald
sets `status: actioned-by-human` — because moving it *is* acting on it).
Reginald also stamps `status_changed_date` whenever the status changes and you
did not date it yourself, and fills in any missing `uuid`, `raised_date` or
`number`.

Statuses:

- **pending** — needs attention.
- **actioned-by-agent** — an agent did the work.
- **actioned-by-human** — a person did the work.
- **ignored** — a deliberate decision to take no action (kept for the record).

## Raising an escalation

Create a file here with the frontmatter above (omit `number`), describe the
issue, and — if you want an agent to act — leave a `[!todo]` or `[!question]`.
Commit and push from your own clone. Reginald adopts it (assigns the number,
mirrors it) on its next run.

## Index

The live list is best read with `reginald escalations list`. This note is the
protocol of record; the folder itself is the index.
