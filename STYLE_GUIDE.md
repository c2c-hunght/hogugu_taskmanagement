# V00600 Ticket Style Guide

Learned from Done/active Business tickets (examples in `reference/style_examples/`).

## 1. Ticket structure in this project

```
Business / Request ticket (biz side, written by PM/BA — Hung, Hoa)
 ├── Sub-task: [Creative] <title>          ← design request (label CreativeTask on parent)
 ├── Link "Biz-Dev" / "Blocks" → Dev Task  ← dev ticket with [BE]/[iOS]/[Android]/[FE]/[QC] sub-tasks
 └── Link "Blocks" → TS2-xxxx              ← Technical Support source ticket (Japanese)
```
- **Business** = feature/change request. **Request** = TS/Ops investigation request. **Task** = dev-owned work.
- Dev sub-task title pattern: `[BE] [Hogugu] <parent title>`, `[QC] Execute test: [Hogugu] <parent title>`.
- Dev posts estimates as a comment: `BE: 3d / iOS: 0.5d / Android: 0.5d / QC: 1d`.

## 2. Title

`[Hogugu][<Scope>][<Scope>] <Short English summary> | <Japanese title (optional)>`

- Scope tags used: `[Customer]`, `[Therapist]`, `[Now]`/`[NOW]`, `[Web]`, `[Dashboard]`, `[BE]`, `[TS]` (from Tech Support), `[Support]`, `[Pro]`.
- Phase work: `... #1`, `... #2` or `... Phase 1`, `Phase 2`.
- Partner-originated tickets append the Japanese title after `|` or `｜`.

Examples:
- `[Hogugu][Web][Now][Customer] Sign-up Improvements #2 to Reduce Drop-offs｜会員登録時の離脱防止のための改修 #2`
- `[Hogugu][TS]Customization settings showing "N/A" for area (Therapist: ...)`

## 3. Description templates

### A. Feature / change (Business)
Reference: V00600-10080, V00600-9946, V00600-10295, V00600-9029

```markdown
## Background
Why. Current situation, who requested (partner / Ops / TS link).

## Purpose            (optional)
Business outcome.

## Current Behavior   (when changing existing behavior)
* ...

## Requirements
### 1. <Sub-feature>
* Rule ...
* **Scope:** <apps>. **Out of scope:** ...

| Screen / Feature | Current | Expect |     ← preferred for UI changes (with screenshots, #100 #101 numbering)

## Scope
* Customer iOS / Android, NOW, Web (booking.hogugu.com incl. INBOUND), Therapist, Dashboard, BE

## Expected Behavior / Acceptance Criteria
* Testable bullets

## Out of scope / Future consideration
* ...

## Reference
WF / Figma / Slack / Confluence links
```

For partner-facing UI tickets, repeat `Background / Requirements / Scope` in Japanese below a `---` (see V00600-8597, 9029).

### B. TS / investigation (Request)
Reference: V00600-11104, V00600-8624, V00600-11181

```markdown
**Source:** Reported via Technical Support ticket TS2-xxxx

## Issue
What was reported, in 2–3 lines.

## Details
* Booking ID / Customer ID / Therapist ID
* Date/time (JST), device, OS, app version
* Timeline

## Expected Result / Actual Result

## Ask
1. Root cause of ...
2. Whether ... is a bug or spec
3. ...

**Priority:** <per source ticket>
```

### C. Dev-owned task (Task)
Reference: V00600-10878 — `Background (policy/reason) → Current status → Action required (numbered) → Affected apps → Deadline`.

## 4. Describe only the change
- Write only what the ticket **changes**. Do not explain unchanged logic (how existing screens are triggered, how existing fields are built, class names of untouched code). It confuses readers about what is in scope.
- "Current Behavior" = the minimum needed to understand the change (e.g. one short table).
- **No implementation details in the spec** (API/service/event names, how data is saved or cached, code-level steps). Check the code to pick the best requirement, then write it as behavior and rules.
- Code findings, implementation hints and cost stay in the local-only `Verification notes` section and are not pushed.

## 5. Formatting conventions
- Headings `##`/`###`, bullets `*`, **bold** for key rules and numbers.
- Put exact UI/notification texts in code blocks (JP original, EN if needed).
- Use tables for Current vs Expect and for CSV/API field specs.
- Mark undecided items `TBD` only in drafts; they must be resolved before Ready For Development.
- Keep the TS reporter's IDs; never include phone numbers.
