# Decisions waiting on Hung — 2026-10-15

Write your answer after each `→ Hung:` line. Local only.

## Urgent

### 1. V00600-11311 — Point up campaign
The October campaign ends **10/31**. Extend again, make permanent, or end? A second (customer) campaign running at the same time is not supported today and needs Dev work.
* Suggest: decide extend / permanent now; split the customer campaign into its own ticket.

→ Hung: (2026-10-15) Current point-up becomes **permanent**, rule unchanged; UI change in a separate ticket. New Dashboard campaign = V00600-11311 draft §1–§3.

## Design approvals (Creative tasks waiting for Hung)

### 2. 7109 — My Rank design (→ 8635)
Waiting since 2025-11.

→ Hung:

### 3. 9810 — Multiple profile images (→ 9808)
Waiting since 2026-06-15.

→ Hung:

### 4. 10443 — Therapist self-registration (→ 10085)
Waiting since 2026-08-03.

→ Hung:

### 5. 10095 — Recommendation list (→ 10084)
Waiting since 07-10.

→ Hung:

## Spec decisions

### 6. V00600-10085 — Registration fields
* Which fields at sign-up: full name, nickname, birthday, email, …? Binh asked on 07-28.
* Does the 30-day first-treatment bonus (11596) start at sign-up or at approval?

→ Hung:

### 7. V00600-8631 — Duplicate
Close it as a duplicate of 10085, after moving the TS2-1381 link to 10085?
* Suggest: yes.

→ Hung:

### 8. V00600-10084 — Recommendation scope
Does Phase 1 replace only the new-therapist slider (as in the ticket and design), or reorder the whole Recommend list (recommender draft)?
* Removing the slider also removes the only exposure boost for new therapists.

→ Hung:

### 9. V00600-8625 — Export
The partner's goal is each therapist's first request, but the list they asked for shows only matched bookings.
* Instead, add "first request / first confirmed date" columns to the existing therapist CSV (~1.5 d)?
* Are phone or other personal-data columns allowed?

→ Hung:

### 10. V00600-8629 — Cancel status labels
* 「セラピキャンセル」 already means "therapist cancelled a confirmed booking" in the Dashboard, but the partner uses it for "therapist declined a request". Which labels?
* How should the 4 automatic cancel reasons plus the 24h no-response cancel be grouped?
* Suggest: Dashboard first; apps as a phase-2 ticket.

→ Hung:

### 11. V00600-8630 — Cancel reason to users
* Channel: the existing admin-cancel push and in-app notification, or chat or email?
* Reuse the existing 「キャンセル理由」 memo, or add a separate reason that users see?
* Same reason for customer and therapist?

→ Hung:

### 12. V00600-8615 — Request notification not received
Link it to 11308 (OneSignal duplicate-subscription fix) and close it after release once Support confirms? The same applies to 8671, 10936 and 8619.
* Suggest: yes.

→ Hung:

### 13. V00600-8637 — Review API
Approve the CTO's existing build (branch `feature/V00600-8637`, PR #214): pool of 20, 4–5★ only, last 180 days, refreshed daily, 4 picked at random. Next: speed check (9926), then merge and QC.
* Suggest: yes.

→ Hung:

## Partner (send them a question)

### 14. V00600-10293 — Rank-change push
We need the thresholds and repeat-notification window for each rank, plus the push texts. Only Silver is given.

→ Hung:

### 15. TS2-1411 / TS2-1249 (→ 8625, 8629)
Both have waited since 2025 for the timing discussion we promised. The partner needs a reply with a date.

→ Hung:

## New drafts ([Q]s are in each file)

### 16. Manual bonus TP — [draft](tickets/drafts/NEW_manual-tp-grant.md)
* Fixed or free amount? Upper limit?
* Block bookings completed over 90 days ago?
* A direct undo action for operators?
* Notify the therapist?
* Which roles can grant?

→ Hung:

### 17. Pre-chat auto-reply — [draft](tickets/drafts/NEW_prechat-auto-reply.md)
* Sender name: the existing one is 「AI自動返信」, or something else?
* Send again after a new 48h inactive period?
* Does the therapist see the message?
* English text?

→ Hung:

### 18. Profile translation — [draft](tickets/drafts/NEW_profile-translation.md)
* Fields: self-introduction only?
* Languages: English only?
* Screens: inbound web only?
* How often: nightly?

→ Hung:
