---
title: Task registry
type: index
part_of: "[[Setup]]"
last_verified: 2026-06-16
changelog:
  - { date: 2026-06-16, agent: eu-uk-regdev-paralegal, note: "Initial filing" }
  - { date: 2026-06-16, agent: eu-uk-regdev-paralegal, note: "Added five new tasks: Draft daily brief, Triage inbox, Key dates digest, Archive sweep, Source verification" }
uuid: 1d61859c-8897-425f-a74a-83b554a993b6
publish: true
---

# Task registry

Each task note is both the operating procedure and the prompt Reginald receives.
Edit the note to change the task. Its `requires` list gives the reading order
for the persona, taxonomy and any task-specific context. Reginald fills in
dates and other run data when it renders the note.

Run reports are filed in [[Setup/Logs/Index|Run logs]].

## Active tasks

| Task | File | Trigger | Frequency | Scope summary |
|---|---|---|---|---|
| Research new developments | [[Setup/Tasks/Research new developments\|Research new developments]] | Scheduled | Daily (weekdays) | Scans monitored regulator sources for new publications; creates merged brief notes; updates topic Index.md and Briefs/Index.md |
| Triage inbox | [[Setup/Tasks/Triage inbox\|Triage inbox]] | Scheduled (after Research new developments) | Daily | Classifies and files material dropped into Inbox/; completes frontmatter; updates topic Index.md |
| Update trackers | [[Setup/Tasks/Update trackers\|Update trackers]] | Scheduled | Weekly | Verifies open consultation status and materials against live regulator sources; updates topic Index.md files |
| Key dates digest | [[Setup/Tasks/Key dates digest\|Key dates digest]] | Scheduled | Weekly (Monday) | Sweeps key_dates frontmatter across active notes; produces forward-looking digest of deadlines and milestones |
| Archive sweep | [[Setup/Tasks/Archive sweep\|Archive sweep]] | Scheduled | Monthly (first Monday) | Moves superseded and withdrawn notes to Archive/; audits Archive/ for incomplete metadata; cleans Index.md tables |
| Source verification | [[Setup/Tasks/Source verification\|Source verification]] | Scheduled | Monthly (third Monday) | Checks verified_against URLs for accessibility and material change; flags stale last_verified dates |
| Action edits | [[Setup/Tasks/Action edits\|Action edits]] | Scheduled / on-demand | Daily (01:00) + any trigger | Actions the `[!todo]`/`[!question]` call-outs contributors leave for Reginald; replies in nested call-outs; resolves escalations |

## Task chain

```
Daily:   Research new developments → Triage inbox
         Action edits (drains the call-out queue; any trigger, any frequency)
Weekly:  Update trackers (independent)
         Key dates digest (reads outputs of daily chain)
Monthly: Archive sweep (first Monday)
         Source verification (third Monday)
```

## Task file template

New task files must follow [[Setup/Tasks/Task template|Task template]]. Key requirements:

- YAML frontmatter including `requires`, `trigger`, `frequency`, `status`
- No restatement of persona or taxonomy rules — task-specific exceptions only
- Explicit **Scope**, **Inputs**, **Process**, **Outputs**, **Concurrency**, and **Escalation** sections
- Every output file must comply with the frontmatter schema and carry a `changelog` entry

## Adding a new task

1. Copy `Task template.md` and rename to `[Task name].md` (verb-first, title case).
2. Complete all sections; delete placeholder text.
3. Add a row to the **Active tasks** table above.
4. If the task is downstream of another, note the dependency in both files under **Concurrency**.

Set the task’s `prompt_file` in Reginald’s configuration to the path of its vault note.
