---
title: Task — Update trackers
type: task
agent: eu-uk-regdev-paralegal
requires:
  - "[[Setup/Persona/Persona File]]"
  - "[[Setup/Taxonomy/Taxonomy - Principles and folder structure]]"
  - "[[Setup/Taxonomy/Taxonomy - Filing conventions]]"
  - "[[Setup/Taxonomy/Taxonomy - Indexes briefs and multi-agent rules]]"
trigger: scheduled
schedule: weekly
frequency: weekly
status: active
last_verified: 2026-06-16
changelog:
  - { date: 2026-06-16, agent: eu-uk-regdev-paralegal, note: "Added frontmatter to conform to task template schema" }
uuid: 91410e71-240f-4398-9dba-bfc58018e90d
publish: true
---

# Task — Update trackers

Maintain consultation rows in topic indexes and milestone records in `Materials/`. **Research new developments** owns new development briefs. Record a filing that needs follow-up in `Escalations/`.

## Consultations

Check the `## Open consultations` section in each topic's `Index.md`. For each entry:

- Verify the deadline and status against the regulator's publication page.
- If the deadline has passed and no response statement is published, move the row to **Closed — awaiting response**.
- If a response statement or final instrument has been published, move the row to **Closed — response published**, link the relevant development note when available, and remove the row once the final instrument is filed in the Developments list.
- If a new consultation has opened since the last update, add a row to the **Open** subsection and link the development brief if it has been filed.

## Materials

Check each file under `Topics/[Topic]/Materials/` for any topic where materials exist:

- Verify that each milestone date and status entry reflects the current position.
- If an amending instrument has been adopted or a provisional date has been confirmed, update the relevant row and note the source.
- Add new rows for milestones published since the last update.
- Update `last_verified` in the frontmatter.
- Ensure the file is linked from the topic's `Index.md`.
