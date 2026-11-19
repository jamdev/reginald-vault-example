---
title: Taxonomy — Principles and folder structure
type: taxonomy-section
section: 1 of 4
part_of: "[[Setup/Persona/Persona File]]"
last_verified: 2026-05-24
uuid: 0aaa47eb-b9d7-422b-82cd-21eca25efff7
publish: true
---

# Vault taxonomy and filing rules (UK/EU fintech)

This vault is an Obsidian-style knowledge base shared across multiple agents. The taxonomy across these four taxonomy files is the canonical filing structure. Agents must follow it for every file they create.

# Operating principles

- **Lazy folder creation.** Do not create a folder until there is at least one substantive file to put in it. Empty scaffolding is forbidden.
- **One source of truth.** A note lives in exactly one folder, classified by its primary regulatory topic. Secondary topics, jurisdictions, and statuses are expressed in YAML frontmatter and tags, not by duplication.
- **Atomic notes.** Prefer one development, instrument, judgment, consultation, or memo per note. Use Obsidian wiki-links to connect them.
- **Primary over secondary.** When a note summarises a development, the underlying primary source (Act, SI, Regulation, Directive, judgment, regulator publication) must be cited and, where licensing allows, linked or cached under `Sources/`.
- **Agent etiquette.** Before editing a note created by another agent, append an entry to its `changelog` frontmatter array rather than rewriting silently. Never delete another agent's note; move it to `Archive/` with a `superseded_by` link.
- **Substantive linkage over narrative linkage.** Co-publication is not coordination. Two instruments published on the same day, in the same press release, or grouped under a regulator's narrative do not by that fact alone share a substantive theme. Use `related:` frontmatter, `Index.md` cross-references, and brief "wider context" sections only where there is a real substantive overlap.

# Top-level folder structure

```
Agentic GC/
├── Setup/              # Persona, taxonomy, task instructions and glossary
│   ├── Tasks/          # One operating procedure and prompt per task
│   └── Logs/           # Task run reports and their index
├── Inbox/              # Untriaged drafts pending classification
├── Briefs/             # Master brief index only (Index.md); brief files live under Topics/
│   └── Index.md        # Reverse-chronological master index linking to all briefs
├── Topics/             # PRIMARY classification by regulatory topic
│   └── [Topic]/
│       ├── Index.md    # Map of Content for the topic; includes open consultations tracking
│       ├── Briefs/     # Merged development-record and action-brief files for this topic
│       └── Materials/  # Implementation timelines, trackers, and other topic-specific materials
├── Sources/            # Cached or linked primary sources (legislation, judgments, guidance)
├── Templates/          # Reusable templates (memo, brief, watchlist entry, etc.)
└── Archive/            # Superseded, expired or withdrawn material
```

Folder names are unprefixed. Obsidian sorts alphabetically by default; the small number of top-level folders makes the order easy to read. Sub-folders inside `Topics/[Topic]/` are similarly unprefixed. The `Briefs/` subfolder under each topic is the standard first sub-folder; `Materials/` is created when there is implementation timeline, tracker, or other reference material to file. Open consultations are tracked directly in the topic's `Index.md`, not in a separate folder. There is no top-level `Trackers/` folder.
