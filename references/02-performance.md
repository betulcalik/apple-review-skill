# 2. Performance — Guideline Section 2

Performance is the largest rejection family by volume in Apple's transparency reports. It contains the #1 practical killer (2.1 — reviewer couldn't use the app) and the metadata accuracy rules that trip automated checks.

## 2.1 App Completeness

### 2.1(a) — final, complete, working app

Rejects when: app crashes or has obvious bugs; placeholder content ("Lorem ipsum", "coming soon", empty feeds, test data, `TODO` screens); broken/dead URLs; login without working demo credentials; backend asleep during review; pre-order app that isn't final.

**Evidence in repo:**

- placeholder greps: `rg -ni "lorem|ipsum|placeholder|coming soon|TODO:|FIXME|test account|demo mode"`
- launch-path fragility: force-unwrapped optionals / `try!` in `AppDelegate`/`SceneDelegate`/root view; synchronous network on main thread at startup
- empty-state design: does the first screen show real content for a fresh user (or a blank screen reviewers read as broken)?
- region/IP-gated content: will a US-based reviewer see anything?
- staged feature flags defaulting off — if the app hides everything behind a flag, reviewers see an empty shell

**Demo mode:** if you legally can't provide a demo account, Apple allows a built-in demo mode *with prior approval* — it must exhibit full features.

### 2.1(b) — IAP visible and functional

Every configured IAP must be findable, complete, and purchasable in the reviewed build; products "not found" during review need an explanation in review notes. Common technical causes: products in "Missing Metadata" state, not submitted with the binary, StoreKit product IDs mismatched, sandbox account region mismatch, receipt-validation server rejecting sandbox receipts.

## 2.2 Beta Testing

No demos/betas/trials on the App Store — use TestFlight. TestFlight builds must be intended for public distribution and comply with the guidelines; no compensation for testers; significant beta updates go through TestFlight App Review first.

**Evidence:** version strings like `0.x`, "beta" in name/metadata, TestFlight-only flags left on.

## 2.3 Accurate Metadata

Metadata must "accurately reflect the app's core experience" and stay current. This is the second-biggest rejection family in practice — and mostly fixable without a new binary (metadata rejection).

| Rule | Check |
|---|---|
| 2.3.1(a) | new features/changes described **with specificity** in Notes for Review — generic descriptions rejected; features must be accessible for review. Misleading marketing (offering what the app doesn't do — e.g. "virus scanner") = removal + possible account termination |
| 2.3.1(b) | egregious/repeated dishonesty = Developer Program removal |
| 2.3.2 | IAP must be clearly indicated in description/screenshots/previews; promoted IAPs need proper display names, screenshots, descriptions and a working `paymentQueue` observer for app-launch purchases |
| 2.3.3 | screenshots show the app **in use** — not title art, login page, splash; overlays to explain input are fine |
| 2.3.4 | previews = video screen capture of the app itself (stickers/iMessage may show Messages experience); narration/overlays allowed |
| 2.3.5 | appropriate category; Apple will change it if you're way off |
| 2.3.6 | honest age-rating questionnaire; local content-rating compliance (film/music/game ratings display) |
| 2.3.7 | unique name, accurate keywords, no trademarked terms/other app names/pricing stuffed into metadata; name ≤ 30 chars; subtitles follow standard metadata rules; no unverifiable claims |
| 2.3.8 | metadata (icons/screenshots/previews) appropriate for **4+ even if app is rated higher**; "For Kids/For Children" reserved for Kids Category; icon/name/screenshots consistent with each other |
| 2.3.9 | rights for all icon/screenshot/preview material; show **fictional** account info, not a real person's data |
| 2.3.10 | no other-platform names/icons/imagery (Android, alternative marketplaces) unless specifically approved; no irrelevant metadata |
| 2.3.11 | pre-order app must ship as advertised; material changes (e.g. business model) restart pre-order sales |
| 2.3.12 | "What's New" must specifically describe significant changes; only bug fixes/security/perf may be generic |
| 2.3.13 | in-app events: accurate metadata, event types from ASC, correct times/dates across storefronts, working deep links, monetization per Section 3 |
| 2.3.14 | **creative assets** (new Oct 2026): must pertain to the app's content/functionality/brand, follow metadata rules, be all-ages appropriate, no misleading references to other apps, unverifiable claims, or specific/discounted pricing |

**Evidence:** these are ASC-side — mark NEEDS-VERIFY, but audit what the repo controls: Fastlane/Snapshot configs, screenshot source folders (title art? login screens?), localized metadata files, app name/subtitle strings, keyword lists, "What's New" strings in the release branch.

**Review-environment detection warning:** code that behaves differently under review (flag flips keyed to reviewer device, sandbox detection, hidden features for later "unlock") violates 2.3.1(a)/2.5.19 and, when discovered, escalates to Code of Conduct territory. Never advise it.

## 2.4 Hardware Compatibility

| Rule | Check |
|---|---|
| 2.4.1 | iPhone apps should run on iPad "whenever possible" — universal apps with broken iPad layouts are a classic 4.0/2.4.1 double rejection |
| 2.4.2 | power efficiency: no battery drain, heat, unnecessary resource strain; no background crypto mining (incl. via ads); no "charge under your pillow" style instructions |
| 2.4.3 | Apple TV apps usable with just the Siri remote; controller-required games must say so in metadata |
| 2.4.4 | never suggest/require device restarts or unrelated system-setting changes ("turn off Wi-Fi", "disable security features") |
| 2.4.5 | Mac App Store extras: sandboxing; Xcode packaging (no third-party installers); no auto-launch at login without consent; no downloading extra code/kexts; no root/setuid; no license screens/keys/copy protection; updates only via MAS; runs on current macOS, no deprecated tech; all localizations in one bundle |

## 2.5 Software Requirements

| Rule | Check | Evidence |
|---|---|---|
| 2.5.1 | public APIs only; current OS; no deprecated frameworks; API used for intended purpose (HomeKit = home automation, HealthKit = health) — and **description says so** | `rg` for private-API patterns, IOKit/undocumented selectors; deployment target vs newest SDK |
| 2.5.2 | self-contained bundle; no reading/writing outside container; no downloading/executing code that changes features (exception: educational code-teaching apps where code is viewable/editable) | JSPatch/CodePush/expo-updates usage; `eval`/`JavaScriptCore` executing remote payloads |
| 2.5.3 | no malware/disruptive code incl. abusing Push Notifications/Game Center | — |
| 2.5.4 | background modes only for intended purpose (VoIP, audio, location, task completion, local notifications) | `UIBackgroundModes` vs actual features — declared-but-unused modes are an automated rejection |
| 2.5.5 | fully functional on IPv6-only networks | hard-coded IPv4 literals, DNS-by-IP, `AF_INET` assumptions |
| 2.5.6 | web-browsing apps must use WebKit/WebKit JS (alt engines only via EU/Japan entitlement) | third-party engine libs |
| 2.5.8 | no alternate home-screen/desktop environments | launcher-style apps |
| 2.5.9 | don't alter/disable standard switches (volume, ring/silent) or native UI behaviors; don't block expected links out to other apps | overrides of system controls |
| 2.5.11 | SiriKit/Shortcuts: only intents users expect from your stated functionality; vocabulary/aliases must relate to your app; no ads between request and fulfillment; disambiguation only when required | intent registration, plist vocabulary |
| 2.5.12 | CallKit/SMS-fraud extensions: only confirmed-spam blocking; criteria explained in marketing; no reuse of that data beyond the feature | — |
| 2.5.13 | facial recognition for auth → LocalAuthentication (not ARKit/custom), with alternate auth for under-13s | face-recognition implementations |
| 2.5.14 | explicit consent + clear visual/audible indicator when recording user activity (camera, mic, screen, inputs) | recording features lacking indicator UI |
| 2.5.15 | file-picker apps must include Files app + iCloud documents | `UIDocumentPickerViewController` modes |
| 2.5.16 | widgets/extensions/notifications must relate to app content & functionality; (a) App Clip features must be in the main binary too; App Clips can't contain advertising | widget code vs app content |
| 2.5.17 | Matter support must use Apple's Matter framework for pairing; non-Apple Matter components must be CSA-certified | Matter SDKs |
| 2.5.18 | display ads only in the main app binary — never in extensions, App Clips, widgets, notifications, keyboards, watchOS apps. In-app ads: age-appropriate, user can see all targeting info without leaving the app, no behavioral targeting on sensitive data (health, school, kids), interstitials clearly labeled as ads with visible close/skip, and users can **report inappropriate ads** | ad SDK presence per target |
| 2.5.19 | **hidden/dormant/undocumented features** (new Oct 2026, split from 2.3.1): functionality must be clear to users and App Review; hiding features via obfuscation = removal + account termination | server-gated feature toggles that change core behavior, encrypted payloads, remote-config-triggered feature unlocks |

**Omitted numbers** (2.5.7, 2.5.10) are intentionally omitted by Apple — old rules (streetscape, etc.). Don't cite them.

## Practical ranking for this section

1. P0: crash-prone launch path / no demo account / dead backend / placeholder screens (2.1)
2. P0: privacy-manifest & upload gate failures (see `upload-errors.md` — technically not 2.x but hit first)
3. P1: metadata mismatches Apple can verify automatically (name/keywords/screenshots/age rating) (2.3)
4. P1: background modes declared but unused (2.5.4), deprecated APIs (2.5.1)
5. P2: iPad layout breakage (2.4.1 + 4.0), ads in extension (2.5.18), review-notes vagueness (2.3.1(a))
