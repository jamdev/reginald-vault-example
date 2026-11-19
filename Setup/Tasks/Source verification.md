---
title: Task — Source verification
type: task
agent: eu-uk-regdev-paralegal
requires:
  - "[[Setup/Persona/Persona File]]"
  - "[[Setup/Taxonomy/Taxonomy - Principles and folder structure]]"
  - "[[Setup/Taxonomy/Taxonomy - Filing conventions]]"
  - "[[Setup/Taxonomy/Taxonomy - Indexes briefs and multi-agent rules]]"
trigger: scheduled
schedule: monthly, third Monday
frequency: monthly
status: active
last_verified: 2026-06-16
changelog:
  - { date: 2026-06-16, agent: eu-uk-regdev-paralegal, note: "Initial filing" }
uuid: 5feb4965-893e-4286-aefd-161245db39cf
publish: true
---

# Task — Source verification

# Purpose

Check `verified_against` URLs in active notes for accessibility and material change, flag stale `last_verified` dates, and ensure each note's factual claims remain supported by its cited sources.

# Scope

**In scope:**
- All active notes under `Topics/` with `status` of `in-force`, `proposed`, `adopted-not-in-force`, `consultation`, or `draft`
- All URLs listed in `verified_against` frontmatter arrays
- Notes where `last_verified` is more than 60 days before the current run date
- Notes with no `verified_against` field or an empty array

**Out of scope:**
- Notes with `status: superseded` or `withdrawn` — these are handled by **Archive sweep**
- `Sources/` cached files — do not re-verify cached text; verify the live source URL in the companion `Source.md`
- Substantive redrafting of note content — this task flags discrepancies; it does not rewrite briefs
- Notes verified within the last 60 days — skip unless a `flag:` field indicates a known source issue

# Inputs

Use `{{VAULT_ROOT}}` as the vault root. For this run on {{TODAY}}, the 60-day cutoff is {{DATE_MINUS_60}}.

| Input | Location | Notes |
|---|---|---|
| Active notes | `Topics/[Topic]/Briefs/` and `Topics/[Topic]/Materials/` | Read frontmatter for `verified_against` and `last_verified` |
| Source companion files | `Sources/[Topic]/Source.md` | Check provenance URL if note cites a cached source |

# Process

**Pass 1 — Identify stale and unsourced notes**

1. Walk all active notes. Build a list of:
   - Notes with `last_verified` more than 60 days ago
   - Notes with no `verified_against` field or an empty array
   - Notes carrying an existing `flag: verify-*` field from a prior task

2. Prioritise in this order: (a) notes with active deadlines in `key_dates` within 30 days; (b) notes with `status: in-force`; (c) all others.

**Pass 2 — Check URLs**

3. For each note in the priority list, retrieve each URL in `verified_against`:
   - **Accessible and unchanged**: update `last_verified` to today's date; append a changelog entry.
   - **Accessible but materially changed** (page content substantively differs from what the note describes): set `flag: source-changed — [date]` in frontmatter; do not update `last_verified`; escalate.
   - **Inaccessible (404, redirect to unrelated page, server error)**: set `flag: source-unavailable — [date]`; attempt to locate the source at an alternative URL (e.g. EUR-Lex by CELEX number, legislation.gov.uk by SI number, Wayback Machine as a last resort); escalate if not resolved.
   - **URL missing or empty**: set `flag: source-missing — [date]`; escalate.

4. For notes with no `verified_against` field at all, attempt to identify the primary source from the note's body text. If identified, add to `verified_against` and verify. If not identifiable, set `flag: source-missing — [date]` and escalate.

**Pass 3 — Output a verification report**

5. Create a report at `Setup/Logs/Source verification - {{TODAY}}.md` listing:
   - Notes verified without issue (count and list)
   - Notes with flags raised (list with flag type and note link)
   - Notes skipped (verified within 60 days)
   - Any URLs that returned errors, with the URL and note

# Outputs

| Output | Location | Action |
|---|---|---|
| Updated `last_verified` field | Active notes where verification passed | Edit frontmatter; append changelog entry |
| `flag:` fields | Active notes where issues found | Edit frontmatter; append changelog entry |
| Verification report | `Setup/Logs/Source verification - {{TODAY}}.md` | Create |

# Concurrency

- This task makes only frontmatter edits (no content changes). Other agents may be reading or editing note content concurrently — do not overwrite body text.
- Read the full file before editing frontmatter to avoid overwriting changelog entries added by another agent in the same window.
- Do not touch `Briefs/Index.md` or topic `Index.md` files — this task does not change a note's classification or status.

# Escalation

- A source URL that was accessible at last verification but now returns an error or redirects to an unrelated page — may indicate the instrument has been repealed or the regulator has restructured its site
- A source whose content appears materially changed relative to the note's summary (e.g. an instrument shown as proposed is now shown as adopted, or vice versa)
- A note with `status: in-force` citing a source that no longer exists
- More than 20 notes with `flag: source-*` in a single sweep — indicates a possible systematic source issue (e.g. regulator site restructure)
- A note where the source change would affect a filing or deadline that is less than 30 days away — escalate immediately rather than waiting for the next daily brief
