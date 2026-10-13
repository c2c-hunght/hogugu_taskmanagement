# HOGUGU Task Management Workspace

Local workspace for preparing Jira tickets in project **V00600** before pushing them to Jira.

- Jira site: https://c2cdoc.atlassian.net (cloudId `52d139c8-37f9-4c69-8afa-4156804d6b98`)
- Board: https://c2cdoc.atlassian.net/jira/software/c/projects/V00600/boards/1482/backlog
- Current batch: open tickets **due 2026-10-30 – 2026-11-01** (20 tickets, scanned 2026-09-29)

## Read first

| File | Purpose |
| --- | --- |
| [RULES.md](RULES.md) | Working rules (push policy, PII, definition of ready) |
| [STYLE_GUIDE.md](STYLE_GUIDE.md) | How V00600 tickets are written (titles, sections, bilingual) |
| [TRACKER.md](TRACKER.md) | Master list of the batch: category, readiness verdict, next action |

## Folder layout

```
hogugu_taskmanagement/
├── README.md / RULES.md / STYLE_GUIDE.md / TRACKER.md
├── tickets/
│   ├── raw/       ← read-only mirror of Jira (description + comments + links). Never edit by hand; re-export instead.
│   ├── review/    ← readiness review for tickets that already have content (gaps + questions)
│   └── drafts/    ← proposed new/updated Jira content. This is what gets pushed after approval.
└── reference/
    ├── related_tickets.md   ← predecessor / related tickets used for context
    └── style_examples/      ← well-written Done tickets used as templates
```

## Workflow

1. **Scan** — export tickets into `tickets/raw/` (description, comments, links, subtasks).
2. **Classify** — in `TRACKER.md`: *Empty* (write together) vs *Has content* (review for readiness).
3. **Review** — for content tickets, write `tickets/review/<KEY>.md` using the checklist in RULES.md.
4. **Draft** — for empty tickets or tickets needing a rewrite, write `tickets/drafts/<KEY>.md` following STYLE_GUIDE.md. Work through open questions with Hung.
5. **Approve** — Hung marks the draft `Status: APPROVED` in its header.
6. **Push** — only on explicit request; update Jira, then log it in TRACKER.md (date + what changed).
