# 4. Design — Guideline Section 4

Design rejections are quality judgments, which makes them feel arbitrary — but the patterns below are what reviewers actually cite. Note the general Design clause (4.0): apps must be polished, usable, and "app-like"; the informal 4.0 family catches broken layouts, unreadable text, tiny touch targets, missing Dynamic Type, and other HIG-level problems that don't fit a numbered rule.

## 4.1 Copycats

- **(a)** Don't copy the latest popular app or minor-variant another app's name/UI — beyond rejection risk, this feeds 5.2 IP claims.
- **(b)** **Impersonation** of other apps/services = Developer Code of Conduct violation, can end in Program removal.
- **(c)** No using another developer's icon, brand, or product name in your icon/name without approval.

Apple publishes a "prevent copycat rejection" help page; the fix is differentiation evidence: unique value proposition, original UI, own brand assets. **Evidence:** app name/subtitle/icon similarity to top charts; template-marketplace provenance of art assets.

## 4.2 Minimum Functionality

The app must elevate beyond a repackaged website. Apps that are just a song/movie → iTunes; just a book/guide → Apple Books.

| Rule | Trigger |
|---|---|
| 4.2 (general) | not useful, unique, or "app-like"; no lasting entertainment/adequate utility; marketing material in app form |
| 4.2.1 | ARKit apps that just drop a model in a view or replay animation |
| 4.2.2 | catalogs are OK-ish, but apps that are primarily marketing/ads/web clippings/content aggregators/link collections are not |
| 4.2.3(i) | app must work standalone — not require installing another app |
| 4.2.3(ii) | if initial-launch downloads are needed, **disclose the size and prompt first** |
| 4.2.6 | commercialized template/app-generator apps must be submitted by the content provider, with real customization tools — or use an aggregated "picker" single-binary model. Agencies mass-submitting client templates get rejected (and can trigger 4.3 spam) |
| 4.2.7 | Remote desktop clients: (a) connect only to user-owned PC/console over **LAN**; (b) execution stays on the host, no extra APIs beyond streaming; (c) account creation from host only; (d) client UI must not resemble iOS/App Store views, no browsing/buying software not owned; (e) **thin clients for cloud apps are not appropriate at all** (cloud-game access outside the allowed streaming-game model → rejected) |

**Evidence for the WebView trap (the most common 4.2 rejection):** app whose screens are mostly `WKWebView` wraps of a responsive site, with minimal native navigation. Count native screens vs web screens; check for pull-to-refresh hacks, no offline handling, no native share integration. Fixes: native navigation/chrome, device-feature integration (camera, share sheet, notifications), content caching — or ship it as a website.

## 4.3 Spam

- **(a)** No multiple Bundle IDs of the same app (one app per city/team/university — use one app with IAP variations).
- **(b)** Nothing indistinguishable from what's already widely available. Explicit saturated lists: **dating, flashlight, sound effects, wallpaper, simple timers, fortune telling** — new submissions must be meaningfully different or rejected. Low-quality/low-effort genres called out: **drinking games, Kama Sutra, fart, burp** apps. Repeated submissions of this kind can escalate to Program removal.

**Evidence:** how many apps from the same account share a codebase (4.3(a)); how similar the app is to category leaders (4.3(b)); white-label/template signals. Note 4.3 is also the guideline Apple applies to AI-generated app floods.

## 4.4 Extensions

Hosting extensions → follow the App/Safari extension programming guides; include functionality like help screens/settings; disclose extensions in marketing text; **no marketing, advertising, or IAP inside extensions**.

- **4.4.1 Keyboards:** must provide input; follow Sticker rules if images/emoji; provide "next keyboard" progression; remain functional **without full access and without network**; collect activity only to enhance the keyboard. Must not launch other apps (Settings excepted) or repurpose keys.
- **4.4.2 Safari extensions:** run on current Safari; don't interfere with System/Safari UI; no malicious/misleading content; request access only to the websites strictly necessary (overly broad permissions = rejection).

## 4.5 Apple Sites and Services

| Rule | Constraint |
|---|---|
| 4.5.1 | approved RSS feeds only; **no scraping apple.com/iTunes/App Store/ASC/developer portal**; no rankings built on that data |
| 4.5.2 Apple Music | MusicKit: users initiate playback, standard controls, no payment/monetization for Apple Music access; no downloading/uploading/sharing music files; deeper integrations need rights-holder licenses; cover art/metadata only for playback/playlists; follow Apple Music identity guidelines; Apple Music user data access must be disclosed in the purpose string and never shared for ads/profiling |
| 4.5.3 | don't spam/phish via Apple services (Game Center, Push, Live Activities); no reverse lookup/mining of Player IDs or aliases |
| 4.5.4 | **Push notifications:** never required for the app to function; no sensitive personal/confidential content; promotions/direct marketing only with explicit opt-in via in-app consent UI **and** an in-app opt-out method |
| 4.5.5 | Game Center Player IDs only per Game Center terms; never displayed or shared |
| 4.5.6 | Apple emoji render in-app/metadata OK; may not be used on other platforms or embedded in the binary |

## 4.7 Mini apps, streaming games, chatbots, plug-ins, emulators

You may offer non-embedded software (HTML5/JS mini apps & games, streaming games, chatbots, plug-ins, retro-game emulators that download games). You are responsible for everything offered (4.7.1–4.7.5 in the sub-rules: IAP and privacy compliance for hosted software, no exposing native APIs to hosted content without permission, universal-link index requirement for hosted software, age restriction for mature mini apps). Games offered this way must be downloaded from the App Store (streaming game services) and the whole catalog is your responsibility.

## 4.8 Login Services (the Sign in with Apple rule)

If the app uses **third-party or social login** (Facebook, Google, X, LinkedIn, Amazon, WeChat…) to set up or authenticate the **primary account**, it must also offer an equivalent option that:

1. limits data collection to name + email,
2. lets users keep their email private, and
3. doesn't collect in-app interactions for advertising without consent.

In practice: offer **Sign in with Apple** (or your own email-login system meeting those tests). "Equivalent prominence" — SIWA hidden below a fold or behind "more options" has been rejected; put it on the same screen, visually comparable.

**Exemptions:** own-account-system-only apps · alternative-marketplace apps using marketplace login · education/enterprise/business apps requiring existing org accounts · government/industry eID apps · clients of a specific third-party service where users must sign into that service (mail client, social network client) for their own content.

**Evidence:** login screen code + entitlements (`com.apple.developer.applesignin` presence when any social login exists). This is a top-5 practical rejection for social apps alongside 1.2 and 5.1.1(v).

## 4.9 Apple Pay

All material purchase info before sale; correct Apple Pay branding/UI per Apple Pay Marketing + HIG. Recurring payments must disclose: renewal term and auto-continuation, what's provided each period, actual charges, and how to cancel.

## 4.10 Monetizing Built-In Capabilities

You may not charge for what the OS/hardware already provides: push notifications, camera, gyroscope, Apple Music access, iCloud storage, Screen Time APIs. (Charging for your *service built on top* is fine; charging to unlock, say, "camera zoom" that just uses the camera is not.)

---

## Design-section audit heuristics

1. Native-to-web ratio of screens (4.2 / 4.0).
2. Template/white-label provenance + number of sibling apps on the account (4.2.6 / 4.3).
3. Login flow inventory → social logins present ⇒ SIWA equivalent present and prominent (4.8).
4. Extension targets: ads/marketing/IAP inside? keyboards full-access? Safari extension host-permissions minimal (4.4)?
5. Push notification usage map: transactional vs marketing; opt-in/opt-out paths (4.5.4).
6. Apple Pay flows: disclosures for recurring charges (4.9). Any paywall for camera/gyro/push/etc. (4.10)?
7. Saturated category (4.3(b)) — if the app is the Nth wallpaper/timer app, the differentiators must be visible in the first screen and the release notes.
