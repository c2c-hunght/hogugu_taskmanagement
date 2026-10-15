<!--
Status: DRAFT (not pushed)
Created: 2026-10-15
Type: Business (Task Type = Requirement definition)
Source: tickets/raw/NEW_profile-translation.md (Hung, one line: 「プロフィールを定期的に翻訳する」)
-->

# [Hogugu][BE][Web][Customer] Translate therapist profiles periodically | プロフィールを定期的に翻訳する

## Background
* Foreign customers (INBOUND web / English app) see therapist profiles **in Japanese only**. Profile text is not translated today.
* We want therapist profiles to be translated automatically and kept up to date.

## Current Behavior
| Item | Current |
| --- | --- |
| Therapist profile (自己紹介 etc.) | Shown as written by the therapist (Japanese) on all platforms, including INBOUND web |
| Translation | Not available for profiles (chat messages only) |

## Requirements
### 1. Periodic translation
* A job runs **periodically** and translates therapist profiles that are new or **changed since the last translation**.
* Unchanged profiles are not translated again.
* Frequency: [Q] daily? (recommended: daily, at night JST)
* First run: all existing therapist profiles are translated (backfill). [Q] Only active/published therapists?

### 2. Target fields and languages
* Fields: [Q] 自己紹介 (introduction) only, or also nickname / menu descriptions / other free-text fields?
* Languages: [Q] English only? (INBOUND web also supports other languages?)

### 3. Display
* When the customer views in a target language (e.g. INBOUND web in English), show the translated profile.
* If no translation exists yet (e.g. profile edited after the last run), show the original Japanese text.
* [Q] Show a note such as 「自動翻訳」 / "Automatically translated"?
* Scope of display: [Q] INBOUND web only, or also booking web and Customer iOS / Android in English?

### 4. Operations
* [Q] Can operators view / correct the translation in the Dashboard? (recommended: out of scope for Phase 1)

## Scope
* BE (periodic job, storage)
* [Q] INBOUND web / booking web / Customer iOS / Android (display)

## Acceptance Criteria
* After the job runs, every target profile has a translation in the target language(s).
* A profile edited by the therapist is re-translated on the next run; unchanged profiles are not re-translated.
* Customers viewing in the target language see the translated text; if missing, the Japanese original is shown.
* Japanese view is unchanged.

## Out of scope / Future consideration
* Manual editing of translations in the Dashboard (unless decided otherwise in §4)
* Translating reviews and chat history

---

## Verification notes (local only — not pushed)
* No profile translation exists today. The introduction is rendered as stored:
  * inbound web `apps/hogugu-web-inbound-booking/src/components/Therapists/PrimaryInfoSection/index.tsx:189` and `Card/index.tsx:147`
  * web booking `PrimaryInfoSection/index.tsx:231`
* Google Cloud Translation v2 is already integrated through the account-api proxy:
  * `servers/hogugu-account-api/app/Services/GoogleProxyService.js:23, 190-225`
  * routes `start/routes.js:757-768`
  * Redis cache 7 days (`:33`), max 5000 chars (`:37`)
  * Used today only on the fly for chat, address and area.
* No `translatedAt` / source-hash fields exist; the model change is needed.
* The introduction is set via `TherapistUpdateProfile` (max 600 chars) and `UpdateIntroductionTherapist` (max 200), `routes.js:504/509`.
* Scheduling pattern: scheduled AWS Lambda → RabbitMQ → listener. Example: `servers/hogugu-booking-api/lambda/weeklyGenerateNewSchedule/index.js:7-26` → `start/rabbitmq.js:181-183` → `app/Listeners/Schedule.js:35-75`. The schedule is probably set in the AWS console (no IaC found).
* Side findings (not part of this ticket):
  * Therapist iOS calls Google directly with its own key (`TextTranslationRequestData.swift:17`), bypassing the proxy.
  * Web booking (customer) calls the `mobile/therapist/...` translate endpoint (`googleTranslate.ts:65`).
  * `NEXT_PUBLIC_GOOGLE_TRANSLATE_KEY` is left in `apps/hogugu-web-booking/example.env:24`.

### Options (for decision; not pushed)
| Option | Pros | Cons | Cost (rough) |
| --- | --- | --- | --- |
| A. Translate on view (client, existing proxy) | Smallest change | Translates on every view (cache 7 days); no SEO; no correction | 1–2 d per client |
| B. Translate on save + store translation | Always fresh; stored; correctable later | Model change + backfill | 3–5 d + backfill |
| C. Periodic job (= the request) + stored translation with source hash | Matches the request; catches failed/old profiles; batch-friendly | Up to 1 day stale after edit; one more scheduled job | 3–5 d BE + display per client |

* Recommendation: **C**, optionally combined with B later.
* Google v2 is ~$20 per 1M chars. A full backfill of introductions (≤600 chars) costs a few dollars per 10k therapists.
