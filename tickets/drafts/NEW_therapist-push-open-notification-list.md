<!--
Status: PUSHED 2026-10-19 as V00600-11755 (Business, Requirement definition, Medium, To Do, unassigned, no due date). Req 1 payload left to Dev. Foreground banner (Req 3) kept.
Created: 2026-10-19
Type: Business (Task Type = Requirement definition)
Source: tickets/raw/NEW_therapist-push-open-notification-list.md (partner via dev channel, Hung 2026-10-19)
Similar: V00600-10761 (Customer app: open therapist profile from push, metadata for Shakebrain)
-->

# [Hogugu][Therapist] Open the notification list (お知らせ) from push notification | Push通知からお知らせ一覧への遷移対応

## Background
* The partner are sending push notifications to therapists (via Shakebrain). When a therapist taps these notification, the therapist app should open the **お知らせ (notification list)** tab.
* Currently the therapist app has no push type that opens the notification list. Tapping a push without booking information just opens the app on the last screen.
* We need to define a push notification parameter that Shakebrain can add to the payload.

## Requirements
### 1. New push parameter
* Shakebrain sends the push **directly through FCM**.
* Please define the push notification parameter (payload format) that opens the お知らせ tab, implement it in the therapist app, and share the payload spec with an example so we can pass it to Shakebrain.
* FCM pushes without this parameter keep the current behavior.

### 2. Tap behavior (app in background / closed)
* Tapping a push with the new parameter opens the therapist app on the **お知らせ tab**, and the list is reloaded so the latest notifications are shown.
* Works the same when the app was closed (cold start) and when it was in the background.
* If an open dialog / sub-screen blocks the tab, close it the same way as the other push types that switch tabs (e.g. the Monday schedule reminder → スケジュール tab).
* If the therapist is not logged in, the login screen is shown as usual. After login, the app opens the home screen (same as other push types).
* During a call screen, nothing happens (same as other push types).

### 3. App in foreground
* The in-app notification banner is shown as for other pushes.
* Tapping the banner opens the お知らせ tab.

### 4. Old app versions
* Old app versions do not know this parameter. Tapping the push only opens the app (no screen change). No error or crash.

## Scope
* Therapist iOS / Android
* No BE change.

## Acceptance Criteria
* The payload spec for Shakebrain is shared in this ticket.
* Tap a push with the new parameter while the app is in the background → the お知らせ tab opens and shows the latest list.
* Same with the app fully closed → after start-up, the お知らせ tab opens.
* Same while on another tab / sub-screen (e.g. booking detail) → the お知らせ tab opens.
* App in foreground → banner is shown; tapping it opens the お知らせ tab.
* Existing push types still open the same screens as before.
* Old app version + new push → the app opens without error, no screen change.




## Verification notes (local only — not pushed)

* Req 1 left to Dev (Hung 2026-10-19). Own suggestion if Dev asks: FCM `notification` (title/body) + `data: { "type": "OpenNotificationList" }` — same `type` key and PascalCase style as existing types; data-only messages are not shown on Android (see below).

Checked 2026-10-19 on local repos: therapist-ios `master` (952720f04, v3.16.1), therapist-android `develop` (c5cface3, v3.16.2). Not pulled — recheck latest branches before push.

* **Payload key `type`** — both apps read the push type from `type` in OneSignal additional data:
  * iOS: `HOReceiveNotification.swift:53` (`info["type"]` → `NotificationType(rawValue:)`; unknown → not matched)
  * Android: `lib/models/one_signal_notification/additional_data.dart:29-34` (`@JsonKey(name: 'type', unknownEnumValue: HONotificationType.other)`)
  * Value style = PascalCase strings (`RemindTherapistScheduleAtMonday`, etc.): iOS enum `HOReceiveNotification.swift:11-26`, Android `lib/constants/enums.dart:140-174`. Customer app uses the same key/style (`NewsAnnouncement`, customer-android `lib/constants/enums.dart:84`).
* **No existing route to お知らせ** —
  * iOS tap routing: `AppDelegate.swift:152` `handleNotificationInfoInBackground` (called from OneSignal `handleNotificationAction`, `AppDelegate.swift:325-341`); default case needs `bookingId` else nothing. `userNotificationCenter(didReceive:)` `AppDelegate.swift:438` only handles the Monday schedule type.
  * Android tap routing: `lib/screens/app_builder_notification.dart:27`, switch at ~:72, default returns when `bookingId == null` (:122). Read-marking by `notificationId` at :38-44.
* **Tab exists** — iOS `TabBarMenu.notification` (`HOMainTabBarViewController.swift:16`), tab switch pattern `appManager().openScheduleManagement()` → `mainTabBar()?.selectTabbarIndex(...)` (`HOAppManager.swift:498`). Android `HOTabItemEnum.Notification` (`lib/screens/main_tabbar/tab_navigator.dart:12`), pattern `BottomTabbarCubit.changeCurrentTabBarTo(...)` (`app_builder_notification.dart` schedule case).
* **Foreground** — iOS `handleNotificationInfoWhenActive` (`AppDelegate.swift:211`) builds `InAppBookingObject` per type; tap handled in `HOAppManager.swift:~120-130`. Android `onReceiveForegroundNotification`. New type needs a branch in both.
* **Old versions safe** — unknown type → iOS `defaultType`/nil path, Android `other` → default case → no `bookingId` → return. No crash expected.
* "Timeline" in Hung's wording = お知らせ一覧 (partner text). In code, "timeline" is the booking timeline in booking detail (`lib/screens/booking_timeline/`) — not this. Customer ticket V00600-10761 also says "timeline screen" for the notification list.
* **Q1 answer (Hung 2026-10-19): Shakebrain sends directly via FCM.** Impact found in code — today both apps route taps only for OneSignal payloads, so a direct FCM push needs a new tap handler:
  * Apps register the FCM token to Hogugu API: iOS `HOAppManager+FCM.swift:26` (`registerFCMToken`), Android `lib/blocs/store_fcm/store_fcm_bloc.dart:52-63`.
  * iOS: `userNotificationCenter(didReceive:)` (`AppDelegate.swift:438-447`) and `willPresent` (`AppDelegate.swift:416-420`) `guard` on `userInfo["custom"]["a"]` (OneSignal format) → a raw FCM payload (data keys at top level of `userInfo`) is ignored. Note `willPresent` returns without calling `completionHandler` → FCM pushes are probably **not shown in foreground today**.
  * Android: the only `MESSAGING_EVENT` service is native `VoiceFirebaseMessagingService` (`android/app/src/main/AndroidManifest.xml:53`), which only handles Twilio payloads and logs/drops anything else (`VoiceFirebaseMessagingService.kt:22-46`). So: background/closed + `notification` message → shown by the OS, tap opens launcher activity with `data` in intent extras (no Dart handler: no `onMessageOpenedApp` / `getInitialMessage` in `lib/`); foreground → dropped, **not shown today**; data-only message → never shown. Hence the spec requires `notification` + `data`.
  * Foreground banner (Req 3) is therefore **new behavior** for FCM pushes on both platforms, not just a new branch. Dev must not break Twilio call pushes (Android service).
* Rough estimate (for Dev comment, not in spec): iOS 0.5–1d / Android 1d (FCM handling + foreground display) / QC 1d.
