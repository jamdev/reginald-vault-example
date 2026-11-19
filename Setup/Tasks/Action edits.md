---
title: Task — Action edits
type: task
agent: eu-uk-regdev-paralegal
requires:
  - "[[Setup/Persona/Persona File]]"
  - "[[Setup/Taxonomy/Taxonomy - Principles and folder structure]]"
  - "[[Setup/Taxonomy/Taxonomy - Filing conventions]]"
  - "[[Setup/Taxonomy/Taxonomy - Indexes briefs and multi-agent rules]]"
  - "[[Escalations/Index]]"
trigger: scheduled
schedule: daily, 01:00 (also on demand, and as a pre-step of every task run)
frequency: daily
status: active
last_verified: 2026-08-10
changelog:
  - { date: 2026-08-10, agent: eu-uk-regdev-paralegal, note: "Initial filing — actions the callout work queue left by contributors" }
uuid: 3c9d8e7f-2a1b-4c6d-8e5f-7a9b0c1d2e3f
publish: true
---

# Purpose

Act on the outstanding `[!todo]` and `[!question]` callouts that contributors
(human or agent) have left addressed to Reginald anywhere in the vault, and
reply in place so the exchange reads as a thread.

# How this task is driven

Pending items for {{TODAY}}

{{MANIFEST}}

**You are given a manifest.** The orchestration layer has already scanned the
vault's git history, identified the pending callout items addressed to you (or
unaddressed), and passed them to you as a numbered list. **Work only from that
manifest.** Do not scan the vault yourself looking for callouts — the manifest
is the single source of truth for what needs doing this run.

This task may run **any number of times per day**, from a timer, from the CLI,
or (in future) from a web request. It is not a once-a-day job. Each run handles
whatever is pending at that moment; anything you do not get to remains queued
for the next run. Never assume you are seeing the whole day's work, and never
assume you are the only run.

# Scope

**In scope:**
- Each callout item in the manifest: read the note and the surrounding thread,
  do the work it asks for (research, verify a source, fix an error, answer a
  question), and reply with the correct nested callout.
- Where an item resolves an escalation, set that escalation file's `status:`
  frontmatter to `actioned-by-agent`.

**Out of scope (do not touch):**
- Moving escalation files between folders — that is done automatically by the
  sync step. You set `status:`; you never move or rename the file.
- Assigning or changing an escalation `number:` — also automatic.
- Callouts not in your manifest, and callouts addressed to another contributor.
- Another task's territory: `Materials/` files and open-consultation rows in
  `Index.md` (Update trackers), Inbox classification (Triage inbox). If a todo
  asks for something in another task's remit, say so in a `[!failure]` or
  `[!warning]` reply rather than doing it.

# Process

1. For each manifest item, open the file at the given path and read the whole
   thread it belongs to, so your reply fits the conversation.
2. Do the work. Use web search and source retrieval where a todo asks you to
   verify something against a primary source — prefer primary regulatory text
   over press releases, per the persona.
3. Reply by **nesting a callout one level under** the callout you are
   answering:
   - `[!success]` when done — title `YYYY-MM-DD — <short outcome>`, then a
     concise account of what you did and what you found.
   - `[!failure]` when you could not — say what you tried and what is needed to
     proceed.
   - `[!question]` back to the human when you are blocked on a decision only
     they can make.
4. If the item was raised in an escalation file and your work resolves it, set
   `status: actioned-by-agent` in that file's frontmatter. Leave the file where
   it is — the sync step moves it to `Resolved/`.
5. Preserve the original callout and append your reply. When it resolves an interim flag or a pending-verification statement, update the surrounding brief or topic index to reflect the outcome. Record procedural details in the run log or the relevant escalation.
6. If a manifest item is unclear or its note has changed out from under it, say
   so in a `[!warning]` reply and move on — do not guess.

# Outputs

| Output | Location | Action |
|---|---|---|
| Nested reply callouts | The note each manifest item lives in | Add; never edit others' callouts |
| Escalation status | `Escalations/[...].md` frontmatter | Set `status:` on resolution; do not move the file |

# Concurrency

- Every run takes the vault lock, so no other task writes while you work.
- Read each note immediately before editing it — a human may have added to the
  thread since the manifest was built.
- A reply on a published brief does **not** email subscribers: the notification
  layer treats callout dialogue as non-content. Say what you need to say freely.

# Escalation

If a todo reveals a problem beyond its own scope — a development that should be
filed, a source that has materially changed, a contradiction between notes —
raise it the normal way: create an escalation file under `Escalations/` (omit
the number; the sync assigns it) describing the issue, and reference it in your
reply.
