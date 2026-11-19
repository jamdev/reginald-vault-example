---
title: Task — Archive sweep
type: task
agent: eu-uk-regdev-paralegal
requires:
  - "[[Setup/Persona/Persona File]]"
  - "[[Setup/Taxonomy/Taxonomy - Principles and folder structure]]"
  - "[[Setup/Taxonomy/Taxonomy - Filing conventions]]"
  - "[[Setup/Taxonomy/Taxonomy - Indexes briefs and multi-agent rules]]"
trigger: scheduled
schedule: monthly, first Monday
frequency: monthly
status: active
last_verified: 2026-06-16
changelog:
  - { date: 2026-06-16, agent: eu-uk-regdev-paralegal, note: "Initial filing" }
uuid: 2c40fc2d-08a1-45b3-8895-135b5e850f76
publish: true
---

# Task — Archive sweep

# Purpose

Identify notes in active folders that should have been archived, move them with correct metadata, and verify that the `Archive/` folder itself is clean and consistent.

# Scope

**In scope:**
- Notes under `Topics/` with `status: superseded` or `status: withdrawn` that have not yet been moved to `Archive/`
- Notes under `Archive/` missing required `superseded_by` or `status` fields
- Stale open consultation rows in topic `Index.md` files where the final instrument has been filed but the row has not been moved to **Closed — response published**
- Brief files that duplicate a superseded development without linking to its replacement

**Out of scope:**
- Notes with `status: in-force`, `proposed`, `adopted-not-in-force`, `consultation`, or `draft` — do not archive active material
- `Sources/` — cached source text is not archived; it stays in place unless it accompanies a superseded note
- Deleting any file — move to `Archive/` only; never delete
- Making substantive judgements about whether a development is superseded — only act on notes whose `status` field already says so, or where a later note explicitly sets `superseded_by`

# Inputs

Work in `{{VAULT_ROOT}}` for the sweep dated {{TODAY}}.

| Input | Location | Notes |
|---|---|---|
| All notes in Topics/ | `Topics/[Topic]/Briefs/` and `Topics/[Topic]/Materials/` | Check `status` frontmatter field |
| Topic Index.md files | `Topics/[Topic]/Index.md` | Check Open consultations for rows that should be closed |
| Archive/ contents | `Archive/` | Check for missing frontmatter fields on already-archived notes |

# Process

**Pass 1 — Find unarchived superseded notes**

1. Walk all files under `Topics/`. For each file where `status` is `superseded` or `withdrawn`:
   a. Confirm the note has a `superseded_by` wiki-link. If missing, check the vault for a likely replacement note. If found, add the field. If not found, set `flag: superseded-by-unknown — [date]` and escalate rather than archiving blindly.
   b. Move the file to `Archive/<original-path>`, preserving the full subfolder structure.
   c. On the archived copy, confirm `status: superseded` (or `withdrawn`) and `superseded_by` are set.
   d. Remove any link to the archived note from its topic `Index.md` active sections. Do not remove it from `Briefs/Index.md` — leave the row with a note that the file is archived.

**Pass 2 — Check Index.md consultation tables**

2. Walk all topic `Index.md` files. In the **Closed — response published** subsection, check whether the linked final instrument has been filed as an active development. If so, remove the row from the table (the development is captured elsewhere). If not, leave the row and flag.

3. In the **Open** subsection, check for rows where the deadline has passed and no response statement is yet published. These should have been moved to **Closed — awaiting response** by **Update trackers**. If found, move the row and append a changelog entry noting the discrepancy.

**Pass 3 — Audit Archive/ for completeness**

4. Walk all files under `Archive/`. For each file missing `status`, `superseded_by`, or `changelog`, add the missing fields where determinable from context. Where not determinable, set `flag: archive-incomplete — [date]`.

**Pass 4 — Output a sweep report**

5. Create a brief sweep report at `Setup/Logs/Archive sweep - {{TODAY}}.md` listing:
   - Files moved to Archive/ this run
   - Index.md rows updated
   - Flags raised
   - Files skipped with reason

# Outputs

| Output | Location | Action |
|---|---|---|
| Archived notes | `Archive/<original-path>` | Moved from Topics/; frontmatter updated |
| Updated topic Index.md | `Topics/[Topic]/Index.md` | Remove links to archived notes from active sections; append changelog |
| Sweep report | `Setup/Logs/Archive sweep - {{TODAY}}.md` | Create; summary of run |

# Concurrency

- Read every file before moving it — another agent may have updated it since the last sweep.
- Append a `changelog` entry to every file touched, including files already in `Archive/`.
- Do not move a file if another agent has edited it within the last 24 hours (check `changelog` last entry date) — wait for the next sweep cycle and escalate.
- Do not edit brief content — only update `status`, `superseded_by`, and `changelog` fields.

# Escalation

- A note marked `superseded` with no identifiable replacement
- A note that appears substantively active (recent `changelog` edits, future `key_dates`) but carries `status: superseded` — possible agent error; do not archive, flag for human review
- More than 10 notes found unarchived in a single sweep — indicates a backlog that may need a dedicated catch-up run
- A file in `Archive/` that appears to have been edited after archiving (changelog entry post-archive date) — flag as `flag: post-archive-edit — [date]`
