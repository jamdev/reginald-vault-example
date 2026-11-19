---
title: Task — Triage inbox
type: task
agent: eu-uk-regdev-paralegal
requires:
  - "[[Setup/Persona/Persona File]]"
  - "[[Setup/Taxonomy/Taxonomy - Principles and folder structure]]"
  - "[[Setup/Taxonomy/Taxonomy - Filing conventions]]"
  - "[[Setup/Taxonomy/Taxonomy - Indexes briefs and multi-agent rules]]"
trigger: scheduled
schedule: daily, after Research new developments
frequency: daily
status: active
last_verified: 2026-06-16
changelog:
  - { date: 2026-06-16, agent: eu-uk-regdev-paralegal, note: "Initial filing" }
uuid: bf59d642-b01a-4f27-a85c-ffd8d8b6f581
publish: true
---

# Task — Triage inbox

# Purpose

Process files dropped into `Inbox/` by completing their frontmatter, classifying them by topic, and moving them to the correct vault location so they are accessible to other tasks and agents.

# Scope

**In scope:**
- All files present in `Inbox/` at the time the task runs
- Completing missing frontmatter fields
- Classifying by primary topic and jurisdiction
- Moving to the correct folder under `Topics/`, `Sources/`, or `Templates/`
- Updating the relevant topic `Index.md` and `Briefs/Index.md` after filing

**Out of scope:**
- Substantive research or verification of content — if a note requires original research to complete, move it to `Inbox/` with a `flag: needs-research` field and escalate rather than drafting content
- Creating new topic folders speculatively — lazy folder creation applies; only create a folder if the topic has substantive content to file
- Files already in `Topics/`, `Briefs/`, `Sources/`, or `Archive/` — do not re-triage filed notes

# Inputs

| Input | Location | Notes |
|---|---|---|
| Untriaged files | `Inbox/` | Process all files present; treat each independently |
| Topic taxonomy | `[[Setup/Taxonomy/Taxonomy - Regulatory topics]]` | Use to assign primary topic |
| Topic Index.md files | `Topics/[Topic]/Index.md` | Read before adding links; check for duplicates |

# Process

For each file in `Inbox/`:

1. Read the file in full.

2. Determine file type from content:
   - Development note or brief → file under `Topics/[Topic]/Briefs/` (merge format if not already merged)
   - Source / cached primary text → file under `Sources/[Topic]/`
   - Template → file under `Templates/`
   - If type is ambiguous, default to brief format and note ambiguity in `changelog`

3. Assign primary topic using `Taxonomy — Regulatory topics`. If genuinely cross-topic, assign the topic that prompted the note and set `secondary_topics`.

4. Assign jurisdiction: `UK`, `EU`, `UK-EU`, or `International`.

5. Complete the YAML frontmatter schema (see Taxonomy — Filing conventions). For any field that cannot be determined from the file content alone, set the value to `unknown` and add a `flag: verify-[fieldname]` entry.

6. Construct the correct filename: `YYYY-MM-DD - Short title - jurisdiction.md`. Use the effective date of the development; if unknown, use today's date and set `flag: verify-effective-date`.

7. Move the file to the target location. Do not leave a copy in `Inbox/`.

8. Update the target topic's `Index.md`:
   - Briefs → add to the **Developments** list (and **Open consultations** if applicable); rewrite the **Summary** within its 3–5 sentence limit
   - Materials → ensure the file is linked under the relevant section

9. If the file is a brief, add a row to `Briefs/Index.md` and reassess the topic's entries in `Home.md` (## Topic index — max three per topic, weighted by recency and importance).

10. After processing all files, confirm `Inbox/` is empty. If any file could not be triaged, leave it in `Inbox/` with a `flag:` frontmatter field explaining the blocker.

# Outputs

| Output | Location | Action |
|---|---|---|
| Filed note | `Topics/[Topic]/Briefs/` or `Sources/[Topic]/` or `Templates/` | Move from Inbox/; full frontmatter required |
| Updated topic Index.md | `Topics/[Topic]/Index.md` | Edit; append changelog entry |
| Updated master brief index | `Briefs/Index.md` | Edit if a brief was filed; prepend new row |

# Concurrency

- Read the target `Index.md` immediately before editing — another agent (e.g. **Research new developments**) may have updated it in the same run window.
- Do not edit `Materials/` files — owned by **Update trackers**.
- If a file in `Inbox/` appears to duplicate a note already filed, do not overwrite. Set `flag: possible-duplicate — [[wiki-link to existing note]]` and escalate.

# Escalation

- Any file that cannot be classified after reading in full
- Apparent duplicate of an existing filed note
- A file whose source cannot be identified (no URL, no citation, no provenance)
- A file that appears to require a new topic folder not currently in the vault

When escalating, leave the file in `Inbox/` with `flag: [reason] — [date]` in frontmatter.
