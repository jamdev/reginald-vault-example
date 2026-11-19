---
title: Task — Key dates digest
type: task
agent: eu-uk-regdev-paralegal
requires:
  - "[[Setup/Persona/Persona File]]"
  - "[[Setup/Taxonomy/Taxonomy - Principles and folder structure]]"
  - "[[Setup/Taxonomy/Taxonomy - Filing conventions]]"
  - "[[Setup/Taxonomy/Taxonomy - Indexes briefs and multi-agent rules]]"
trigger: scheduled
schedule: weekly, Monday morning
frequency: weekly
status: active
last_verified: 2026-06-16
changelog:
  - { date: 2026-06-16, agent: eu-uk-regdev-paralegal, note: "Initial filing" }
uuid: 17916a46-0f94-4494-a66f-ceec65ffb703
publish: true
---

# Task — Key dates digest

# Purpose

Sweep `key_dates` frontmatter fields across all active notes and produce a forward-looking digest of upcoming deadlines, effective dates, and implementation milestones, sorted chronologically and grouped by topic.

# Scope

**In scope:**
- All notes with `status` of `in-force`, `proposed`, `adopted-not-in-force`, `consultation`, or `draft`
- All `key_dates` entries with a date falling within the next 90 days (T+90 window)
- Consultation deadlines tracked in topic `Index.md` open consultations tables

**Out of scope:**
- Notes with `status: superseded`, `withdrawn`, or `enforcement` — exclude unless the enforcement action carries a compliance deadline
- Dates more than 90 days out — include in a separate **Beyond 90 days** section only if flagged `status: provisional` and likely to shift
- Updating the underlying notes — this task reads only; corrections to dates belong to **Update trackers** or **Research new developments**

# Inputs

Work in `{{VAULT_ROOT}}`. The 90-day window runs from {{TODAY}} through {{DATE_PLUS_90}}.

| Input | Location | Notes |
|---|---|---|
| All active notes | `Topics/[Topic]/Briefs/` and `Topics/[Topic]/Materials/` | Read frontmatter `key_dates` field |
| Topic Index.md consultation tables | `Topics/[Topic]/Index.md` | Extract deadline column from Open consultations table |
| Previous digest | `Topics/` (most recent digest file) | Read to identify dates that have shifted or passed |

# Process

1. Walk all files under `Topics/` with a `status` field that is not `superseded` or `withdrawn`.

2. For each file, extract:
   - All entries in `key_dates[]` where `date` falls within T+90
   - The `title`, `topic`, `jurisdiction`, `status`, and a short label for each date entry

3. Walk all topic `Index.md` files and extract open consultation deadlines from the **Open** subsection of the consultations table.

4. Merge the two sets of dates. Deduplicate where a consultation is captured both in a brief's `key_dates` and in the `Index.md` table.

5. Sort all entries chronologically. Group into the following sections:
   - **This week** (T+7)
   - **This month** (T+8 to T+30)
   - **Next 60 days** (T+31 to T+90)
   - **Beyond 90 days** (provisional dates only, flagged as such)

6. Within each section, group by topic.

7. For each date entry, note:
   - The label (e.g. "DORA full application")
   - The date and whether it is `provisional` or confirmed
   - The linked note
   - Jurisdiction

8. Identify any dates from the previous digest that have now passed. Flag whether they were met, shifted, or remain outstanding.

9. Save the digest to `Topics/[primary topic or cross-topic location]/Materials/Key dates digest - {{TODAY}}.md`. Where the digest is genuinely cross-topic (which it usually will be), file under the topic with the most entries and set `secondary_topics` to all others. Update that topic's `Index.md` to link to the new digest file under **Materials**.

10. If a prior digest file exists for the same period, supersede it: move to `Archive/` and set `superseded_by`.

# Outputs

| Output | Location | Action |
|---|---|---|
| Key dates digest | `Topics/[Topic]/Materials/Key dates digest - {{TODAY}}.md` | Create; full frontmatter required |
| Updated topic Index.md | `Topics/[Topic]/Index.md` | Edit to link new digest under Materials; append changelog entry |
| Archived prior digest | `Archive/Topics/[Topic]/Materials/` | Move if superseding a prior file |

# Concurrency

- This task is read-only across all notes except the digest output file and its linking Index.md entry.
- Do not edit any brief, development note, or Materials file other than the digest itself.
- If the digest output topic's `Index.md` has been recently edited by another agent, read in full before appending.

# Escalation

- A key date that has passed with no corresponding update in the linked note (no `changelog` entry, no status change) — flag as `flag: date-passed-unverified — [date]` in the digest
- A date marked `provisional` that is now within 14 days and has not been confirmed — flag for human review
- More than five dates falling in the same week — flag as a high-density week in the digest header
- A note whose `key_dates` field is missing entirely but whose content references specific implementation dates — flag for **Update trackers** to correct
