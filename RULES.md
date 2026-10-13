# Working Rules

## 1. Push policy
- Everything is prepared **locally first**. Nothing is written to Jira (description edits, comments, transitions, new tickets, links) until Hung explicitly asks to push.
- **Comments are never posted automatically.** "Push" means the description/ticket only. A comment is posted only when Hung explicitly asks for that comment (e.g. "post the comment"). Draft comment text locally and show it first.
- Push only drafts whose header says `Status: APPROVED`. Push one ticket at a time and log it in TRACKER.md.
- Do **not** add an `## Original request` section to pushed descriptions. The original text stays in the local copy (`tickets/raw/`) and in Jira history. If the original contains details (IDs, texts, partner wording) that matter, work them into the new requirements instead.
- Never change assignee, due date, sprint, or status unless asked.

## 2. Confidentiality / PII
- Customer and therapist **phone numbers, emails, addresses, card info** must not be copied into drafts, comments, or chat. Local raw copies have phone numbers redacted.
- IDs (booking ID, customer ID, therapist ID) are OK to reference — they are how Dev investigates.
- Names: prefer IDs; include a name only if the original ticket already does and it is needed.
- Internal links (Slack, Confluence, Figma) stay internal; do not paste them anywhere outside Jira/Confluence.

## 3. Definition of Ready (checklist for review)
A Business/Request ticket is **ready for development** when all of these are answered:

| # | Check | Question to ask |
| --- | --- | --- |
| R1 | Background / why | Why are we doing this? Who asked (partner, Ops, TS, Dev)? |
| R2 | Scope – platforms | Which apps: Customer iOS/Android, NOW, Web (booking / inbound), Therapist iOS/Android, Dashboard, BE? |
| R3 | Current vs expected behavior | Is the current behavior described and the expected behavior testable? |
| R4 | Rules & edge cases | Error cases, empty states, permissions, limits, existing data / migration |
| R5 | Texts & language | Final JP text given? EN text needed (inbound / multi-language)? |
| R6 | Design | Figma/WF linked, or explicitly "no design needed"? Creative subtask done? |
| R7 | Dependencies | External (Shakebrain, OneSignal, GMO, LINE), other tickets, release order (BE before app) |
| R8 | Out of scope | What is explicitly NOT included (phase 2, future consideration) |
| R9 | Acceptance criteria | Bullet list QC can test against |
| R10 | Open questions closed | No "TBD", "(To be continue)", or unanswered questions in comments |

For **investigation (TS/bug) tickets**, the checklist is instead:
- Reproduction info: IDs, date/time (JST), device/OS/app version, screenshots
- Expected vs actual
- Clear ask: "find root cause" vs "fix" vs "confirm spec"
- After investigation: the ticket must be updated with the **decision** (fix / spec / won't fix) before moving to development.

### Verdicts
- **READY** — can go to Dev as-is.
- **READY (minor)** — small clarifications, can be answered in a comment.
- **NEEDS UPDATE** — content exists but has real gaps / is outdated; description must be revised.
- **NEEDS DECISION** — blocked on a business/PM decision (usually Hung), not on writing.
- **EMPTY** — must be written from scratch (together with Hung).

## 4. Verify against source — do not guess
- Every statement about **current behavior, data fields, screens, APIs, or existing integrations** must be checked in the current source (`../servers/`, `../apps/`) before it goes into a draft.
- Each draft ends with a `## Verification notes (local only — not pushed)` section listing what was checked, with `path:line` references.
- If something cannot be confirmed in code, write it as `[Q]`, not as a fact.

## 5. Writing rules
- Follow STYLE_GUIDE.md. English first; add a Japanese section when the ticket is partner-facing.
- One ticket = one deliverable. If a draft grows beyond one release, propose a split.
- Mark unknowns explicitly as `[Q]` in drafts so they can be resolved before push; no `[Q]` may remain at approval.
