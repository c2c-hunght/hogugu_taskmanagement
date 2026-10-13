# Requirements: Therapist Recommendation System | セラピスト推薦システム

Status: DRAFT v0.1 (2026-10-06) | Owner: Hung | Not a Jira ticket. Tickets are created after this document is approved.
Open items are marked `[Q]`. None may remain at approval.

## 1. Background and goal

* Customers choose a therapist from a list. The list order is rule-based and the same logic for every customer (see 3).
* Goal: show each customer therapists who fit them, to raise booking conversion and repeat rate.
* [Q] Requested by whom, and the target KPI and baseline (booking conversion, repeat rate, NOW acceptance rate)?

## 2. Scope

| Item | In scope (Phase 1) | Later / out of scope |
| --- | --- | --- |
| Object | Therapists | Menus, time slots, coupons |
| Surface | Therapist list "Recommend" order [Q: also home / NOW?] | Push / LINE recommendations |
| Apps | BE, Customer iOS / Android, Web [Q: NOW, inbound] | Therapist side, Dashboard |
| Customers | Logged-in customers with booking history, plus a fallback for new customers | - |

## 3. Current state (verified in source)

* Default list order ("Recommend | Favorite") is a fixed multi-key sort: consecutive free slots (up to 6) → earliest free slot → approval rate → response time → response rate → review count → fewer free slots → rating → newest account. Other sorts: price asc / desc, review count. `servers/hogugu-matching-service/app/Repos/AvailableTherapistRepo.js:391-462`
* The order is **not personalized**: no customer attribute or history is an input.
* Therapist rank (from points) is attached to results, not used in the sort. `app/Services/TherapistService.js:232`
* A "random therapists" endpoint by minimum rank exists. `servers/hogugu-account-api/app/Controllers/Http/Common/Customer/TherapistController.js:92`
* Customer app already sends Firebase Analytics events: view therapist, view therapist list / search, sort list, select menu, add to cart, begin checkout, request, rebook, change therapist, submit review. `apps/hogugu-customer-android/lib/services/firebase_analytic/`
* Not confirmed: whether these events are stored where Dev can query them, and whether Web / iOS send the same events. [Q]

## 4. Functional requirements

### FR-1 Personalized order
* The default "Recommend" order of the therapist list is personalized per customer.
* Availability for the requested date / time and area remains a **hard filter**. A therapist who cannot be booked is never ranked above one who can.
* Favorite therapists and previously booked therapists are handled as today [Q: boost, or keep fixed position?].

### FR-2 Signals
Ranking uses, at minimum:

| Group | Signals |
| --- | --- |
| Customer | Past bookings (therapist, menu, duration, area, time of day), ratings given, favorites, cancellations |
| Therapist | Rating, review count, approval / response rate, rank, menu and price, area, gender [Q], new therapist flag |
| Context | Distance to customer, free time near the requested time, booking type (Standard / NOW) |

### FR-3 Customer segments (K-means)
* Customers are grouped into segments from behavior (booking frequency, price band, menu type, time of day, area).
* New customers and customers with little history get therapists popular in their segment or area.
* Segments are recalculated on a schedule [Q: daily / weekly]. K is chosen by offline evaluation (elbow / silhouette) and set by configuration.
* Segment data is also available to Dashboard / marketing [Q: needed?].

### FR-4 Cold start and fairness
* New therapists get a limited exposure boost for the first [Q] days or bookings so they can collect reviews.
* No therapist is hidden by the recommendation. Ordering only.
* Results are diverse: not all top results from the same therapist group [Q: rule].

### FR-5 Fallback
* If the recommendation is unavailable or slow, the list uses the current order (section 3). The customer sees no error.

### FR-6 Event logging (prerequisite)
* Impression, click, booking request, booking completed, cancel and review events are stored with customer ID, therapist ID, list position, sort used and timestamp.
* Same events on Customer iOS / Android and Web.

### FR-7 Experiment
* Customers are split into control (current order) and test (recommended order). Split is stable per customer.
* Results are compared on the KPI from section 1.

## 5. Non-functional requirements

| Item | Requirement |
| --- | --- |
| Response time | List API p95 not worse than today [Q: current value] |
| Freshness | Segment and model refresh on a schedule; availability always real time |
| Explainability | Dev / Ops can see why a therapist was ranked (main signals) |
| Privacy | Behavior data used only for recommendation; confirm consent and retention (PDPC) [Q] |
| Operations | Rollback to the current order by a configuration switch, no release |

## 6. Solution options (for decision)

| # | Option | Pros | Cons | Rough cost |
| --- | --- | --- | --- | --- |
| 1 | Rules / popularity (+ geo, availability) | Fast, explainable, no cold start | Not personalized | 1–2 weeks |
| 2 | Content-based (customer preference vs. therapist attributes) | Works with little data | Narrow results | 2–4 weeks |
| 3 | Collaborative filtering | Finds non-obvious matches | Cold start, needs volume | 4–6 weeks |
| 4 | Hybrid: candidates + re-ranker | Best quality | Feature pipeline, evaluation | 2–3 months |
| 5 | Deep / embedding retrieval | Scales | Overkill for current volume | 3+ months |
| 6 | Bandit on top of 1–4 | Adapts, helps new therapists | Needs traffic | +3–4 weeks |

**K-means (FR-3)** is a building block for 1–4: simple and explainable, works with sparse data, no new infrastructure (about 1–2 weeks). Weak points: one cluster per customer, K must be chosen, coarser than CF.

## 7. Proposed phases

| Phase | Content | Exit criteria |
| --- | --- | --- |
| 0 | FR-6 event logging, baseline KPI | Events stored for [Q] weeks |
| 1 | Option 1 + K-means segments (FR-1, 3, 4, 5, 7) | A/B test shows no loss, target uplift [Q] |
| 2 | Add collaborative filtering / embeddings as a second candidate source | Offline metrics beat Phase 1 |
| 3 | Re-ranker on candidates (hybrid) | A/B uplift on booking conversion |

## 8. Acceptance criteria (Phase 1)

* A customer with history sees an order different from a new customer for the same area and time, and the order is stable between two opens within the same session.
* Unavailable therapists are never ranked above available ones.
* A new customer sees a segment / area-based order, not an error or an empty list.
* Turning the switch off restores the current order immediately.
* Control and test groups are logged and comparable per KPI.
* List response time is within the agreed limit.

## 9. Open questions

1. Requester, KPI and baseline numbers.
2. Which screens and apps get it first (list only, or home / NOW / inbound).
3. Handling of favorite and already-booked therapists.
4. Whether paid / priority placement or Ops pinning must override the ranking.
5. Where event data is stored today and its volume (bookings, reviews, customers).
6. Infrastructure: can scheduled jobs and a cache be added on the current stack (MongoDB, Redis, RabbitMQ, Elasticsearch / OpenSearch)?
7. Privacy: consent wording and retention for behavior data.

---

## Verification notes (local only)

* Checked: `AvailableTherapistRepo.js` sort cases; `TherapistService.js` rank attach; `TherapistController.js` random therapists; customer-android analytics method and constants.
* Not checked: iOS / Web event parity, where Firebase events are exported, data volume, whether `NOWTherapistController` uses the same order, any favorites handling beyond the `favorite_asc` flag in `AvailableTherapistRepo.js:190`.
* Cost figures are rough, for 1–2 devs, from general practice, not from team velocity.
