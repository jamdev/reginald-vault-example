---
title: Taxonomy — Indexes, briefs, trackers, sources, and multi-agent rules
type: taxonomy-section
section: 4 of 4
part_of: "[[Setup/Persona/Persona File]]"
last_verified: 2026-08-13
uuid: f5f0bfec-e137-4c1f-8209-07d93050fc42
publish: true
---

# Index pages (MOCs)

Each topic folder contains a `Index.md` (Map of Content) with the following sections, in order:

1. **Title + scope line** — a single italic sentence describing coverage.
2. `## Summary` — a short prose summary (3–5 sentences maximum) covering all recent developments filed under the topic. Do not let this grow into a long paragraph; when adding a development, rewrite the summary to stay within the limit, dropping detail that lives in the linked briefs.
3. `## Developments` — a single reverse-chronological list of ALL briefs filed under the topic, one bullet per brief: `**YYYY-MM-DD** — [[Topics/<Topic>/Briefs/<brief filename>|<brief title>]] — one-line gist` (plus any ESCALATION or source-quality flag in bold/italic). **Link every brief by its full vault path with an alias**, not a bare `[[filename]]` — bare-name wikilinks do not resolve on Obsidian Publish. This is the only list of briefs in the index — do not maintain separate "Active developments" and "Briefs" lists.
4. `## Materials` — only where a `Materials/` folder exists.
5. `## Open consultations` — table as specified below. Omit the "Closed" subsections entirely when empty rather than showing "(none)".
6. `## Key dates to watch`
7. `## Watchlist`
8. `## Cross-references` — includes any "See also" cross-topic items. Link other topic indexes by full path, e.g. `[[Topics/Payments and E-money/Index|Payments and E-money]]`, never a bare ambiguous `[[Index]]`.

Agents must update the relevant `Index.md` whenever they add, supersede, or archive a note in that topic.

**An index is a living know-how map, not a task log.** It records the *current* state of a topic, not the history of how it got there. When something resolves — a consultation closes, a flagged issue is fixed, an escalation is actioned, a status moves from proposed to in-force, a "pending verification" note is confirmed — **remove** the now-obsolete interim tracking rather than leaving it to accumulate. Do not leave running status commentary, stale ESCALATION/flag markers, or superseded summary lines behind once they no longer describe the present. The audit trail lives in each note's `changelog` frontmatter and in `Setup/Logs/`; the index itself should read as current know-how a professional can trust at a glance. The same applies inside a brief: when a point it raised is resolved, fold the outcome into the note's current state and drop the interim back-and-forth.

**Procedural notes belong in `Escalations/` and `Setup/Logs/`, never in a brief or index.** Run-by-run narration (what was or wasn't fetched this run), source-retrieval mechanics (HTTP errors, JS-rendered pages, "not retrieved this run"), vault-gap tracking, escalation/ESC references, "filed by the X run", and next-run to-dos for other tasks are all bookkeeping, not know-how — record them in `Escalations/` (for anything needing follow-up) or the per-run log under `Setup/Logs/`. A brief's Source quality and Gaps sections may still state a substantive limit (e.g. "based on the press release; secondary sources only"), but not the run mechanics behind it.

Keep frontmatter changelogs on index pages to roughly the 3 most recent entries; older entries are removed when adding a new one (full run records live in `Setup/Logs/`, one file per run).

## Home.md topic index

The vault-level `Home.md` contains a `## Topic index` section with one `###` heading per topic (linking to the topic's `Index.md` by full path) followed by that topic's **two or three** most important recent developments, one line each, weighted by recency and importance. Whenever an agent files or materially updates a brief, it must reassess the topic's Home.md entries: add the new development if it outranks an existing one, and drop the least important entry so the count never exceeds three. Home.md carries headlines only — no summaries, consultations tables or watchlists.

# Briefs

## Where brief files live

Brief files live inside the relevant topic folder:

`Topics/[Topic]/Briefs/YYYY-MM-DD - Short title - jurisdiction.md`

The naming convention follows the standard vault convention (see Taxonomy — Filing conventions): the date is the effective date of the underlying development, not the date the brief was written.

## Merged brief format

Each brief file is a single merged document combining the full development record with the action memo. There is no separate development note; the brief IS the development record. A merged brief contains, in order:

1. YAML frontmatter (`type: brief`, `brief_mode: research` or `incident` as appropriate)
2. Jurisdiction, date and status
3. Summary of development
4. Practical implications
5. Internal context (firm-specific matters, prior correspondence — may be brief if not yet searched)
6. Key considerations (action-specific observations not captured under practical implications)
7. Gaps / what is not verified
8. Recommended next steps
9. Source quality
10. Key sources
11. Caveats

Do not create a separate topic development note and a separate brief note for the same development. Merge them at the point of creation.

## Master brief index

`Briefs/Index.md` is the master reverse-chronological index linking to every brief in the vault, regardless of topic. Agents must update `Briefs/Index.md` whenever they add, supersede, or archive a brief. The `Briefs/` folder contains only `Index.md`; no brief files live directly inside it.

**Link every brief by its full vault path with an alias** — `[[Topics/<Topic>/Briefs/<brief filename>|<brief title>]]` — never a bare `[[filename]]`. Bare-name wikilinks render but do not resolve on the published Obsidian site (regdev.agenticgc.uk); the full path is required there, exactly as for the index-to-index links elsewhere in this document. The alias keeps the displayed text clean.

## Daily and incident briefs

Daily briefs and incident briefs, which are inherently cross-topic, are also filed under the most relevant topic's `Briefs/` subfolder and linked from `Briefs/Index.md`. Where a daily or incident brief covers multiple topics equally, file it under the topic that prompted its creation and cross-reference the others via `secondary_topics` frontmatter.

# Consultations (tracked in topic Index.md)

Open consultations are tracked directly in the `## Open consultations` section of the relevant topic's `Index.md`. Do not create a separate top-level `Trackers/` folder or a standalone consultations file.

The consultations table in each `Index.md` uses the following columns: Authority | Title | Type | Published | Deadline | Linked note. It has three subsections: **Open**, **Closed — awaiting response**, and **Closed — response published** — include the closed subsections only when they have content. Move rows between subsections as status changes; remove rows once the final instrument is published and the development is captured in the Developments section.

# Materials (under Topics/[Topic]/Materials/)

Topic-specific reference materials — implementation timelines, live trackers, open-items lists — are filed in `Topics/[Topic]/Materials/`. Each file follows the standard naming convention where a date is meaningful, or a descriptive title otherwise. Every file in `Materials/` must be linked from the topic's `Index.md`.

Suggested files, created only when there is material to track:

- `Topics/AI and Algorithmic Systems/Materials/Implementation timeline - EU AI Act.md`
- `Topics/Cybersecurity and Operational Resilience/Materials/Implementation timeline - DORA.md`
- `Topics/Crypto-assets and Digital Assets/Materials/Implementation timeline - MiCA.md`
- `Topics/Consumer Protection and Conduct/Materials/UK Consumer Duty - open items.md`

# Sources (under Sources/)

Mirror the topic structure inside `Sources/` but store only the cached primary text (PDF, HTML or extracted markdown) plus a `Source.md` companion describing provenance, retrieval date, and any licence constraints. Never edit cached source text; corrections live in companion notes.

# Multi-agent concurrency rules

- An agent that intends to edit an existing note must read it first, append a `changelog` entry, and preserve any unfamiliar frontmatter fields.
- An agent that adds a new note must populate the full frontmatter schema; missing fields block downstream agents that filter on them.
- An agent that supersedes a note moves the old note to `Archive/<original-path>` and sets `status: superseded` plus `superseded_by: [[wiki-link]]` on the archived copy.
- An agent that disagrees with another agent's classification must not silently re-file; instead add a `reclassification_proposed` field with rationale and surface the conflict in the next daily brief.
- An agent that raises an escalation (a new file under `Escalations/`) must **omit** `number:` and must not invent an `ESC-NNN` id — the orchestrator assigns the number on sync. A self-chosen number collides with one already in the register and is reassigned.
