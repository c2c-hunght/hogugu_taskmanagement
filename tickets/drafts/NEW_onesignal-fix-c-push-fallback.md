# [Hogugu][Therapist][BE] Push notification: send by External ID with fallback to stored device ID (Fix C)

## Background
Some therapists do not receive push notifications because the backend holds a wrong or Unsubscribed OneSignal device ID for them (duplicate subscriptions created by an old OneSignal SDK bug, and kept after reinstall).
Investigation and Fix A/B: [V00600-10936 Confluence](https://c2cdoc.atlassian.net/wiki/x/CYCmLwE) / TS: V00600-10936 / Dev: V00600-10938.

Long-term, therapist pushes will be targeted by OneSignal **External ID** (= therapist ID) instead of the stored device ID. Devices are linked to their External ID by the app update (`login` on app start) and a one-time backfill. This ticket covers the **safety net while devices are still being linked**.

## Purpose
* Therapists linked to an External ID receive pushes on all their active devices, even if the stored device ID is wrong.
* Therapists not linked yet keep receiving pushes exactly as today.

## Requirements
### 1. Send to therapists by External ID first
* For every therapist push, send to the therapist's External ID (= therapist ID).
* Recipient eligibility does not change: a therapist who is blocked, or who has no registered device today, still receives no push.

### 2. Fallback to the stored device ID
* If OneSignal reports that a therapist's External ID is not linked to any device, send the same push again to that therapist's stored device ID(s), as today.
* Fallback is per therapist: in a push to many therapists, only the unlinked ones are re-sent.
* A therapist must not receive the same push twice.
* If the External ID send fails for a reason other than "not linked" (e.g. OneSignal error / timeout), fall back to the stored device ID(s) for all recipients of that push.

### 3. Logging (to decide when the fallback can be removed)
* For each push, log: notification type, number of recipients, number sent by External ID, number sent by fallback, and the therapist IDs that used the fallback.
* Please make the fallback rate visible in Grafana (Loki query or dashboard panel).

### 4. Switch
* Please make this behavior switchable by environment config (ON / OFF) so we can roll back to the current behavior without a code change.

## Scope
* BE (notification service) — therapist pushes only.
* **Out of scope:** Customer, NOW, Admin / Agent pushes; app changes (`login` on app start); the one-time backfill script; removing the fallback (separate ticket once the fallback rate is near 0).

## Expected Behavior / Acceptance Criteria
* Therapist linked to External ID, stored device ID is Unsubscribed → push is received on the Subscribed device.
* Therapist not linked to External ID, valid stored device ID → push is received (via fallback), once only.
* Push to multiple therapists, some linked and some not → every therapist receives the push exactly once.
* Blocked therapist → no push, same as today.
* Switch OFF → behavior identical to today.
* Logs show External ID / fallback counts per push.

## Questions
* [Q] Please confirm the therapist OneSignal app is already migrated to the User Model (required for External ID). If not, this ticket is blocked.
* [Q] Please confirm on staging how OneSignal reports "External ID not linked" (single and multiple recipients, all-unlinked case) and share the response samples.
* [Q] Please confirm External ID set from the iOS therapist app (SDK 2.11.2, `setExternalUserId`) can be targeted the same way as Android.
* [Q] Please share an estimate: BE / QC.

## Reference
* Confluence: https://c2cdoc.atlassian.net/wiki/x/CYCmLwE
* OneSignal: https://documentation.onesignal.com/docs/en/aliases , https://documentation.onesignal.com/reference/create-message

---

<!-- LOCAL ONLY — do not push below this line -->
## Verification notes (2026-11-13)
* All pushes go through `pushHelper.sendNotificationWithPlayerIds` (servers/hogugu-notification-service/app/Helpers/pushHelper.js:13), body `include_player_ids`. ~54 call sites pass `playerIds` only, from `InstallationRepo.getBasedOnUserId` (returns `{ playerIds, userIds }`, already filters `isBlocked`).
  → Implementation needs userIds at send time: either change the helper signature (touch call sites) or have the helper look up userIds from playerIds. Dev decision.
* `onesignal-node` 2.1.1 posts to `https://onesignal.com/api/v1/notifications` and passes the body unchanged → `include_aliases` + `target_channel: "push"` can be added without upgrading the lib. Confirm the legacy endpoint accepts `include_aliases` (else call `api.onesignal.com/notifications`).
* Expected "unlinked" signal: response `errors.invalid_aliases.external_id[]`; when no recipient is valid, OneSignal may return an empty `id` + error. To be confirmed on staging (Q2).
* Installations are MongoDB (`installations` collection), not SQL.
* Therapist iOS: OneSignal pod 2.11.2. Therapist Android: onesignal_flutter 5.x.
* Rough cost: BE 0.5–1d + QC 0.5–1d.
