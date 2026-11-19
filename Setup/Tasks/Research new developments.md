---
title: Task — Research new developments
type: task
agent: eu-uk-regdev-paralegal
requires:
  - "[[Setup/Metadata/Metadata]]"
  - "[[Setup/Persona/Persona File]]"
  - "[[Setup/Taxonomy/Taxonomy - Principles and folder structure]]"
  - "[[Setup/Taxonomy/Taxonomy - Filing conventions]]"
  - "[[Setup/Taxonomy/Taxonomy - Indexes briefs and multi-agent rules]]"
trigger: scheduled
schedule: daily, weekdays
frequency: daily
status: active
last_verified: 2026-06-16
changelog:
  - { date: 2026-06-16, agent: eu-uk-regdev-paralegal, note: "Initial filing" }
  - { date: 2026-08-10, agent: eu-uk-regdev-paralegal, note: "Renamed from Draft daily brief — the task researches and files developments; it does not draft the digest email" }
uuid: 0d8b4e80-af03-47b8-be37-32aac0157237
publish: true
---

# Task — Research new developments

# Purpose

Scan monitored regulator publication pages for new material, assess relevance against active topics, and create a merged brief note for each material development.

# Scope

**In scope:**
- New publications from monitored regulators since the last run (legislation, guidance, consultations, enforcement notices, speeches with policy content, final rules)
- Any topic covered by `Topics/` in the vault
- Cross-topic developments (filed under the primary topic with `secondary_topics` frontmatter)

**Out of scope:**
- Updating consultation status rows in Index.md — handled by **Update trackers**
- Updating Materials/ implementation timelines — handled by **Update trackers**
- Processing files already in Inbox/ — handled by **Triage inbox**
- Retrospective verification of existing notes — handled by **Source verification**

# Inputs

| Input | Location | Notes |
|---|---|---|
| Regulator publication pages | See monitored sources below | Check for publications since last run date |
| Topic Index.md files | `Topics/[Topic]/Index.md` | Read to assess whether a development is already filed |
| Briefs master index | `Briefs/Index.md` | Read to check for duplicate coverage |

## Monitored sources

Check each source for new publications. Prioritise primary legislative/regulatory text over press releases.

**UK**
- FCA: fca.org.uk/news (policy statements, consultations, final notices, guidance)
- PRA: bankofengland.co.uk/prudential-regulation (policy statements, supervisory statements, consultations)
- Bank of England: bankofengland.co.uk/news (MPC/FPC/PRC statements, discussion papers)
- PSR: psr.org.uk/news (policy statements, market reviews, directions)
- CMA: gov.uk/cma (market investigations, merger decisions, enforcement)
- ICO: ico.org.uk/about-the-ico/media-centre (guidance, enforcement, consultations)
- HM Treasury: gov.uk/government/organisations/hm-treasury (consultations, legislation, policy papers)
- legislation.gov.uk: new SIs in financial services, payments, data, AI scope

**EU**
- EUR-Lex (new OJ publications): eur-lex.europa.eu/oj/direct-access.html
- European Commission: ec.europa.eu/newsroom/just (financial services, data, AI)
- EBA: eba.europa.eu/publications (final guidelines, consultation papers, opinions, Q&A)
- ESMA: esma.europa.eu/press-news (guidelines, Q&A, opinions, supervisory convergence)
- EIOPA: eiopa.europa.eu/publications
- ECB/SSM: bankingsupervision.europa.eu/press/publications
- EDPB: edpb.europa.eu/news (guidelines, opinions, enforcement decisions)

# Process

1. Use {{TODAY}} as the run date. Check for publications since {{PREV_RUN_DATE}}.

2. For each monitored source, retrieve the publications page and identify items published since the previous run date.

3. For each new item, assess relevance:
   - Does it fall within a topic covered by `Topics/`?
   - Is it substantive policy content? Exclude purely administrative items (meeting schedules, staff appointments, procurement notices, routine data releases with no policy signal). Include annual reports and work programmes from monitored regulators where they contain substantive supervisory priorities, enforcement data, regulatory simplification positions, or forward signals on regulatory direction.
   - Is it already filed in the vault (check `Briefs/Index.md` and the relevant topic `Index.md`)?

4. For each relevant, unfiled development:
   a. Determine the primary topic from `Taxonomy — Regulatory topics`.
   b. Retrieve and read the primary source text (legislation, guidance document, consultation paper). Do not rely on press releases alone.
   c. Draft a merged brief following the format in Taxonomy — Indexes, briefs, trackers, sources, and multi-agent rules (§ Merged brief format).
   d. Save the brief to `Topics/[Topic]/Briefs/YYYY-MM-DD - Short title - jurisdiction.md` using the effective date of the development, not today's date.
   e. Update the topic `Index.md`: add the development to the **Developments** list (reverse-chronological, one line); rewrite the **Summary** to reflect it while staying within 3–5 sentences.
   f. Add a row to `Briefs/Index.md` linking to the new brief.
   g. Update the topic's entry in `Home.md` (## Topic index): add the development if it outranks the existing entries, keeping at most three per topic (weighted by recency and importance).

5. If a development is cross-topic, set `secondary_topics` in frontmatter and add a cross-reference line in each secondary topic's `Index.md` under a **See also** section.

6. Replace any superseded entry in the topic index with the current position. Record source-retrieval problems and other run details in `Setup/Logs/`. Raise anything needing follow-up in `Escalations/`.

7. If there are no material new developments, record that outcome in the run log and skip creating a brief. At the end of the run, update `last_run_updater` in `Setup/Metadata/Metadata.md` to {{TODAY}}, including on days with no new briefs.

# Outputs

| Output | Location | Action |
|---|---|---|
| New brief file(s) | `Topics/[Topic]/Briefs/YYYY-MM-DD - Title - jurisdiction.md` | Create; full frontmatter required |
| Updated topic Index.md | `Topics/[Topic]/Index.md` | Edit; append changelog entry |
| Updated master brief index | `Briefs/Index.md` | Edit; prepend new row(s) to reverse-chronological list |

# Concurrency

- Read `Briefs/Index.md` and the relevant topic `Index.md` before writing to check for concurrent edits by another agent.
- Do not edit `Topics/[Topic]/Materials/` files — those are owned by **Update trackers**.
- If a topic `Index.md` has been edited by another agent in the same run window, read the full file before appending; do not overwrite their changes.

# Escalation

Flag in the brief and surface prominently if:
- An enforcement action names a firm or individual by name
- A development has a compliance deadline within 30 days
- A development materially contradicts an existing filed note (raise `reclassification_proposed` per taxonomy rules)
- A primary source cannot be retrieved (URL returns error or content appears altered)
- A development falls outside all existing topics and may require a new topic folder

When escalating, set `flag: [reason] — [date]` in the brief's frontmatter.
