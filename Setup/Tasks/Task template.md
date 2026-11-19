---
title: Task — [Task name]
type: task
agent: eu-uk-regdev-paralegal
requires:
  - "[[Setup/Persona/Persona File]]"
  - "[[Setup/Taxonomy/Taxonomy - Principles and folder structure]]"
  - "[[Setup/Taxonomy/Taxonomy - Filing conventions]]"
  - "[[Setup/Taxonomy/Taxonomy - Indexes briefs and multi-agent rules]]"
trigger: scheduled          # scheduled | on-demand | downstream
schedule:                   # human description or cron; omit if on-demand
frequency:                  # e.g. daily, weekly, after each new brief filed
status: active              # active | paused | draft
last_verified: YYYY-MM-DD
changelog:
  - { date: YYYY-MM-DD, agent: eu-uk-regdev-paralegal, note: "Initial filing" }
uuid: 8d61730e-b24b-4620-a17c-50b0e3e50a5b
publish: true
---

# Purpose

One sentence: what this task exists to do and why it recurs.

# Scope

**In scope:**
- [List what this agent reads, checks, or produces]

**Out of scope (do not touch):**
- [List related areas this agent must not modify — handled by another task or agent]

# Inputs

| Input | Location | Notes |
|---|---|---|
| [e.g. Open consultations table] | `Topics/[Topic]/Index.md` | Read only |
| [e.g. Regulator publication page] | [URL pattern or source folder] | Verify against live source |

# Process

Step-by-step instructions the agent follows, in order. Be specific about conditions and decisions.

1. [First step]
2. [Second step — include what to check and what triggers the next action]
3. [...]

If a condition is met that falls outside these steps, go to **Escalation** below before taking any action.

# Outputs

| Output | Location | Action |
|---|---|---|
| [e.g. Updated consultation row] | `Topics/[Topic]/Index.md` | Edit in place; append changelog entry |
| [e.g. New brief] | `Topics/[Topic]/Briefs/YYYY-MM-DD - Title - jurisdiction.md` | Create; update Briefs/Index.md |
| Run log (**every run**) | `Setup/Logs/YYYY-MM-DD - task-name.md` | Create one file per run; add a row to `Setup/Logs/Index.md` |

Every file created or edited must: (a) comply with the YAML frontmatter schema in Taxonomy — Filing conventions; (b) carry a `changelog` entry for this run; (c) be linked from the relevant `Index.md`.

# Concurrency

See Taxonomy — Indexes, briefs, trackers, sources, and multi-agent rules for the full multi-agent concurrency rules. Task-specific notes:

- [Any additional note specific to this task, e.g. "This task reads but does not write Topic Index.md files — leave edits to the Update trackers task."]
- If a file has been edited by another agent since this task last ran, read the changelog before overwriting any section.

# Escalation

Do not resolve the following silently — surface in the next daily brief and, where urgent, flag immediately:

- [Condition 1: e.g. A consultation deadline has passed with no response statement and no regulator update visible]
- [Condition 2: e.g. A source URL returns 404 or the page content appears materially changed]
- [Condition 3: e.g. A classification conflict with another agent's note]

When escalating, add a `flag:` field to the relevant note's frontmatter with a short description and the date.
