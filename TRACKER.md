# Tracker — V00600 tickets due end of Oct 2026

Scanned: 2026-09-29. Filter: `project = V00600 AND duedate in [2026-10-20, 2026-11-01] AND statusCategory != Done` → 20 tickets.
Verdict legend: see RULES.md §3.

## A. Empty / near-empty → write together (drafts in `tickets/drafts/`)

| Key | Title | Type / Status | Assignee | Current content | Draft |
| --- | --- | --- | --- | --- | --- |
| V00600-11308 | [Customer][Therapist][Now] Improve Onesignal PlayID handle | Business / To Do | - | "fix bug" → **PUSHED 2026-09-29 14:44** | [draft](tickets/drafts/V00600-11308.md) |
| V00600-11309 | Securities issues - Oct task | Business / To Do | - | "semgrep -200 isues" → **PUSHED 2026-09-29 14:34** | [draft](tickets/drafts/V00600-11309.md) |
| V00600-11310 | [Web][Now][Customer] Sign-up Improvements | Business / To Do | - | VN one-liner: fix sign-up UI (buttons/text/labels) | [draft](tickets/drafts/V00600-11310.md) |
| V00600-11312 | [Therapist] Show nearest station for NOW booking | Business / To Do | - | **PUSHED 2026-09-29 14:23** (no open [Q]) | [draft](tickets/drafts/V00600-11312.md) |
| V00600-11351 | Semgrep issue fixing | Task / To Do | Binh | empty (5 sub-tasks exist) | [draft](tickets/drafts/V00600-11351.md) |
| V00600-9813 | [BE] Hogugu Server's log improvement | Business / To Do | Binh | 1 issue only → **PUSHED 2026-09-29 14:49** | [draft](tickets/drafts/V00600-9813.md) |

Deferred by Hung (2026-09-29) — content to be added later: 11308, 11309, 11310, 11351, 9813.

Note: 11309 and 11351 overlap (both = Semgrep fixes). Proposal: 11309 = Business umbrella, 11351 = Dev task linked via Biz-Dev.

## B. Has content → readiness review (in `tickets/review/`)

| Key | Title | Type / Status | Assignee | Verdict | Main gap | Review |
| --- | --- | --- | --- | --- | --- | --- |
| V00600-10878 | [Android] Target API 36 | Task / To Do | **unassigned** | READY — **URGENT** | Hard Google deadline Nov 1; no owner yet | [review](tickets/review/V00600-10878.md) |
| V00600-8622 | Customer push text for review | Business / Ready For Dev | Hung | READY (minor) | EN text? which trigger | [review](tickets/review/V00600-8622.md) |
| V00600-10474 | Therapist rank popup | Business / To Do | Hung | READY (minor) | Design sub-task open; platforms (NOW/Web?) | [review](tickets/review/V00600-10474.md) |
| V00600-10990 | Pre-request chat message after block/delete | Request / To Do | Hung | READY (minor) | Final text not chosen; EN text | [review](tickets/review/V00600-10990.md) |
| V00600-9805 | My coupon Phase 1 | Business / Sprint | Binh | READY (minor) | SB API detail "TBD"; auto-apply rule | [review](tickets/review/V00600-9805.md) |
| V00600-11138 | Booking without credit card | Business / Sprint | - | NEEDS UPDATE → DRAFT 2026-10-05 | Jira desc already has 0-yen rules; only small additions; payment-error handling = V00600-11617 (created 10/05); 11138 additions kept local | [draft](tickets/drafts/V00600-11138.md) / [11617](tickets/drafts/V00600-11617.md) |
| V00600-10435 | My coupon Phase 2 | Business / Sprint | Binh | NEEDS UPDATE | Overlaps Phase 1 req #1; delta unclear | [review](tickets/review/V00600-10435.md) |
| V00600-9477 | Push notifications logic improvement | Business / To Do | Nha | NEEDS UPDATE | Description ≠ agreed design (Confluence); status-mgmt debate open | [review](tickets/review/V00600-9477.md) |
| V00600-8632 | NOW request notification/popup | Business / To Do | Hung | READY (minor) | TBD: show popup to therapists who already received NOW (partner) | [review](tickets/review/V00600-8632.md) / **PUSHED 2026-10-02 11:52** (WF/popup/guide-page text in [wf](tickets/drafts/V00600-8632_wf.md)) [draft](tickets/drafts/V00600-8632.md) |
| V00600-11181 | LINE notification timing mismatch | Request / Sprint | Yuta | NEEDS DECISION | Investigation done (11182); fix/spec decision not in ticket | [review](tickets/review/V00600-11181.md) |
| V00600-8624 | NOW "This booking cannot be accepted" | Business / To Do | Hung | DECIDED → fix requested | Fix in description 2026-09-29 (re-check before notify, NOW immediate only); waiting Dev estimate | [draft](tickets/drafts/V00600-8624.md) |
| V00600-8741 | AI auto-reply without customer message | Request / To Do | Hung | DECIDED → fix requested | Fix request in description 2026-09-29 (Web/iOS: mark-suspicious after save); waiting Dev estimate | [draft](tickets/drafts/V00600-8741.md) |
| V00600-8636 | Therapist app error (Redmi) | Business / Pending | Hung | NEEDS DECISION | Root cause unknown since 2025-07; tracking or close | [review](tickets/review/V00600-8636.md) |
| V00600-8633 | Wrong therapist avatar in chat/noti | Business / Ready For Dev | Minh | NEEDS UPDATE | Investigation only; no fix spec; stale (2025-09) | [review](tickets/review/V00600-8633.md) |

## D. New tickets (not in Jira yet)

| Draft | Title | Status |
| --- | --- | --- |
| [V00600-11363](tickets/drafts/V00600-11363.md) | No TP for same customer × therapist within 12 h | **CREATED 2026-09-29** — To Do, Long, due 2026-10-30 |
| [V00600-11596](tickets/drafts/V00600-11596.md) | 初施術ボーナス +6TP (within 30 days of account creation) | **CREATED 2026-10-05** — To Do, Long, due 2026-10-16; **UPDATED 2026-10-06 11:22** (campaign 10/15〜12/31, start after PROD release, account created in period, old-app UI by Dev) |
| [recommender-system](requirements/recommender-system.md) | Therapist recommendation system — **requirements doc first**, tickets after approval | **DRAFT v0.1 2026-10-06** — local only; 7 open questions; current sort verified in code |

## C. Cross-cutting observations
1. **Due date 2026-10-30 looks like a bulk placeholder** on 17 tickets, incl. items blocked since 2025. Realistic plan needed — 5 tickets are waiting on Hung's decision.
2. **Stale TS tickets (2025)**: 8624, 8633, 8636, 8741 — decide fix / close before end of Oct.
3. **Notification cluster**: 9477, 8632, 8624, 11181, 11308, 8622 all touch push/notification. Consider sequencing (9477 infra first) and one regression test pass.
4. **Coupon cluster**: 9805 → 10435 → 11138 (points/coupon/card interaction). Review together.

## Push log
| Date | Key | Action |
| --- | --- | --- |
| 2026-09-29 14:15 | V00600-11312 | Description replaced (original kept under "Original request"); summary → `[Hogugu][Therapist][NOW] Show nearest station for NOW booking \| NOW予約に最寄り駅を表示`. 4 [Q] left open in ticket (list 1 station?, popup 1 vs 3, 20 min threshold, 3 s delay). Assignee (Long), status, due date untouched. |
| 2026-09-29 14:23 | V00600-11312 | Description replaced again: 3 stations on all screens; bus format station→bus→stop→walk, best 3 by bus (one bus, no transfer), booking/current time; 20 min + 3 s confirmed; "Original request" section removed. No [Q] left in ticket. Summary/assignee/status/due date untouched. |
| 2026-09-29 14:34 | V00600-11309 | Description replaced (Hung's wording + 3 typo fixes): October version-up of low-impact APPSEC findings, Director confirms classification list, out-of-scope planned in November, test with October version. Title/assignee (none)/status/due date untouched; 11351 not linked. |
| 2026-09-29 14:36 | V00600-8741 | Comment added (id 222410) to Minh: code check result (Web/iOS call mark-suspicious in parallel with Firestore save; Flutter OK) + fix request "call mark-suspicious only after save succeeds" + 3 AC + estimate request. Description/title/assignee/status/due date untouched. |
| 2026-09-29 14:40 | V00600-8741 | Description replaced: Source TS2-1478, Issue (reservation 195450817049180, 2025/12/17 18:45), Investigation result (Web/iOS/Flutter), Request (Web/iOS: mark-suspicious only after save succeeds), 3 AC. No "Original request" section. Title/assignee/status/due date untouched. Comment 222410 still exists (no delete tool) — Hung to delete manually if needed. |
| 2026-09-29 14:44 | V00600-11308 | Description replaced: request only (keep latest OneSignal ID per device on server in 5 cases; logout / account switch / invalid IDs; please share detailed design before development). Hung's wording + 1 grammar fix. Title/assignee (none)/status/due date untouched. |
| 2026-09-29 14:49 | V00600-9813 | Description replaced: recheck RabbitMQ therapist last-login sync log loop (STG/PRD) + share result + fix if still happening; 3 AC. Title/assignee (Binh)/status/due date untouched. |
| 2026-09-29 | V00600-11363 | **Created** (Business, Task Type = Requirement definition, To Do, assignee Bui Thanh Long, due 2026-10-30): TP 12 h restriction same customer × therapist; history labels new/old app; rank explanation page bullet; JP summary. |
| 2026-09-29 14:46 | V00600-8624 | Description replaced: Source TS2-1412, Issue (booking 124781423211302, therapist IDs), Investigation result, Request (NOW request shown/pushed only while therapist still meets conditions; accept check unchanged; NOW immediate only, time-specific keeps V00600-10762 rule), 5 AC, Scope BE + Therapist app. Customer name/phone removed from description (still in Jira history). Title/assignee/status/due date untouched. |
| 2026-10-02 11:48 | V00600-8632 | Description replaced: Background, Req 1 guide popup (「次回から表示しない」 per device, shown again if unchecked, before request popup, 「閉じる」), Req 2 guide page (Rank-page style image, entry = popup link + フリー予約 settings page), Req 3 Creative 11341/11342, Wireframe & Texts (WF-1/WF-2, JP only, page content ①–⑥: 今すぐ 20 min / 日時指定 45 min before start, no penalty for decline/no response, bonus line campaign-only), Scope, AC. 1 TBD left (existing therapists — partner). Title/assignee/status/due date/links untouched. |
| 2026-10-02 11:52 | V00600-8632 | Description replaced (text update): popup #102 now 4 bullets — #102-2 「今すぐ予約は、リクエストから20分以内に予約を受けられます。」, new #102-3 「日時指定は、施術開始の45分前まで予約を受けられます。」; guide page ② row → 「予約を受けられる期限」 (20分以内 / 施術開始の45分前まで). Nothing else changed. |
| 2026-10-05 | V00600-11596 | **Created** (Business, Task Type = Requirement definition, Medium, To Do, assignee Bui Thanh Long, due 2026-10-16): 初施術ボーナス +6TP — once per therapist, completed (通常会計) by 23:59 JST of day 30 from account creation (day 1 = creation day), 指名+フリー, stacks with +1/+4, permanent from release (BE+Dashboard+apps same time), pre-release therapists included, labels 「初施術ボーナス（登録30日以内）」/「付与取消(初施術ボーナス)」. No label, no links. |
| 2026-10-05 | V00600-11617 | **Created** (Business, Task Type = Requirement definition, Medium, To Do, unassigned, no due date): 決済エラー発生時の対応 — payment error = initial payment failure at completion (changed: completes instead of blocking) / extension unpaid (points short, card auto-charge failed) / cancel fee failed; operator email to info@, Dashboard filter + mark paid + reminder push (confirm + last sent); therapist push, 「決済エラー」 status, whole reward + TP on hold, chat open, not 施術中; customer auto push, request + withdrawal blocked with messages; no time limit. Link: Relates V00600-11138. 11138 description NOT changed (additions kept local). |
| 2026-10-06 11:22 | V00600-11596 | Description replaced: campaign 2026-10-15 〜 12-31 JST (start set after PROD release, no retroactive grant, configurable); target = アカウント作成日 (Dashboard) within period; late-period accounts keep full 30 days; example 10/15 → 11/13; stacking = normal TP + 1TP NOW + future bonuses (+4 removed); old app versions: no UI bug, Dev to share display. "Permanent" and "pre-release therapists" removed. Comment 222737 to Binh/Long: 30 days = calendar days JST, day 1 = creation day; account creation = Dashboard アカウント作成日. Title/assignee/status/due date untouched. |
| 2026-10-06 11:30 | V00600-10474 | Description spelling fix only: "Scrope" → "Scope", "Favrotiest list" → "Favorites list", broken "#### Expected Behavior" heading joined. No content change. Title/assignee/status/due date untouched. No comment. |
