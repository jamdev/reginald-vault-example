---
title: Taxonomy — Filing conventions (naming, frontmatter, tags)
type: taxonomy-section
section: 3 of 4
part_of: "[[Setup/Persona/Persona File]]"
last_verified: 2026-08-12
uuid: d133c108-0c09-4a0a-9dc2-a39d5c490f29
publish: true
---

# File naming convention

`YYYY-MM-DD - Short title - jurisdiction.md`

Examples:

- `2025-11-19 - Digital Omnibus on AI - EU.md`
- `2026-05-07 - Council Parliament AI Omnibus political agreement - EU.md`
- `2024-07-12 - FCA Consumer Duty - UK.md`

Jurisdiction codes: `UK`, `EU`, `UK-EU` (covers both), `International` (FATF, Basel, ISSB, OECD).

The date is the effective date of the development itself (publication, adoption, judgment, entry into force), not the date the note was written. For ongoing or undated topics, use `YYYY-MM-DD` of the most recent material update.

# Required YAML frontmatter

Every note must open with a YAML block. Minimum schema:

```yaml
---
title: Digital Omnibus on AI Regulation Proposal
type: development            # development | judgment | consultation | guidance | memo | brief | tracker | template | source
uuid: 6f1e0c9a-4b2d-4c8e-9a3f-2d7b8e5c1a04   # UUIDv4; permanent identity of this note
jurisdiction: EU             # UK | EU | UK-EU | International
topic: AI and Algorithmic Systems
secondary_topics: []         # array of other topics touched
status: proposed             # in-force | proposed | adopted-not-in-force | consultation | superseded | withdrawn | draft | enforcement
source_type: primary         # primary | official-guidance | consultation | judgment | secondary | internal
effective_date: 2025-11-19
key_dates:
  - { label: "High-risk Annex III obligations apply", date: 2027-12-02, status: provisional }
  - { label: "Article 50 transparency obligations apply", date: 2026-12-02, status: provisional }
authorities: [European Commission, Council of the EU, European Parliament]
instruments: [COM(2025) 836, CELEX:52025PC0836, Regulation (EU) 2024/1689]
tags: [eu, ai, fintech, omnibus, proposed]
last_verified: 2026-05-21
verified_against: [https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX:52025PC0836]
owner_agent: eu-uk-regdev-paralegal
related: []                  # array of wiki-links to related notes
changelog:
  - { date: 2026-05-21, agent: eu-uk-regdev-paralegal, note: "Initial filing" }
---
```

# Note UUIDs

Every note in the vault carries a `uuid` frontmatter field: a version 4 UUID that is the note's permanent identity, independent of its filename, title, or location.

Rules:

- **Every new file** must be created with a freshly generated UUIDv4 in its frontmatter. This applies to all note types — briefs, indexes, trackers, templates, reports, digests.
- **Exactly one `uuid` per note.** A note carries a single `uuid` line. Write it once when creating the note; when editing a note that already has a uuid, leave it untouched — never add a second `uuid` line. Duplicate frontmatter keys are invalid YAML: a parser silently keeps only one, and a later divergent edit to the other silently wins.
- **Never change** an existing note's `uuid`, including when renaming, moving, superseding, or archiving the note. The uuid travels with the file.
- **Never copy** a uuid from one note to another. When splitting or duplicating a note, the new file gets a new uuid.
- If a note is found genuinely without a uuid, add one (a permitted frontmatter-only edit for any task) — but never add a uuid to a note that already has one.
- Downstream systems (orchestration database, subscriber notifications, cross-references) prefer the uuid over the filename; the filename remains the human-readable identifier.

# Tag conventions

Tags are flat and lowercase. Use them for cross-cutting filters that frontmatter fields do not already cover.

- Jurisdiction: `#uk`, `#eu`, `#international`
- Status: `#in-force`, `#proposed`, `#consultation`, `#superseded`
- Source type: `#primary`, `#secondary`, `#guidance`, `#case-law`
- Theme: `#fintech`, `#payments`, `#ai`, `#crypto`, `#privacy`, `#aml`, `#sanctions`, `#consumer-duty`, `#dora`, `#mica`, `#mifid`, `#gdpr`

Do not duplicate topic folder names as tags unless the note crosses topics.
