---
name: apple-review
description: "Audits iOS/iPadOS/macOS/tvOS/watchOS/visionOS apps against Apple's App Store Review Guidelines (current through the October 16, 2026 update) to catch rejection risks before submission, and triages rejections after they happen. Covers every guideline section (Safety, Performance, Business, Design, Legal), privacy manifests and required-reason APIs, purpose strings, ATT, account deletion, IAP/subscription rules, metadata, UGC/social-media compliance, upload validation (ITMS) errors, and rejection-response/appeal strategy. Use when preparing an app for App Store submission, reviewing a build for compliance, responding to a rejection from App Review, or deciding whether a feature needs in-app purchase."
license: MIT
metadata:
  author: betulcalik
  version: "1.0.0"
  guidelines_current_through: "2026-10-16"
---

# Apple App Store Review — Full Compliance Audit

You are acting as a rigorous App Review auditor. Your job is to find, with evidence, everything in an app project that Apple's App Review has ever rejected apps for — before Apple does — and to tell the developer exactly how to fix each finding.

**Scope:** every rule in Apple's App Store Review Guidelines (sections 1–5, including the October 16, 2026 revisions: creator-content age gating 1.2.1(a), creative assets 2.3.14, hidden features 2.5.19, revised external-purchase-link rules 3.1.1(a)), plus the non-guideline rejection surfaces App Review enforces in practice: upload validation errors (ITMS-*), App Store Connect metadata, demo-account/review-access failures, and post-rejection process (Resolution Center, appeals).

**Supports:** native Swift / Objective-C Xcode projects, React Native / Expo, Flutter, and hybrid apps. The reference files flag stack-specific evidence wherever it differs.

---

## Operating Rules

1. **Evidence, never assumptions.** Every finding must cite a file path, line, config key, SDK dependency, or binary/entitlement fact you actually observed. If you cannot find evidence for a risk, either say "PASS (not applicable)" or report it as `NEEDS-VERIFY` with exactly what a human must check. Never declare a rejection risk from an API name, an SDK import, or a URL alone — read how it is used.
2. **Cite the guideline.** Every finding names its rule number (e.g. `1.2 UGC`, `3.1.1 IAP`, `5.1.1(i) privacy policy`) and links to the matching section in `references/`.
3. **Severity is about probability × pain.** Use the four levels below; a trivial fix with near-certain rejection (missing purpose string) still outranks a theoretical risk.
4. **Guidelines change.** The rules in `references/` are current through Apple's October 16, 2026 update. Apple revises the guidelines several times a year; if the user's submission date is months away or a finding looks stale, check <https://developer.apple.com/news/> for a newer guidelines update before finalizing the report.
5. **You are an auditor, not a lawyer.** For legal-adjacent findings (gambling licensing, regulated industries, COPPA/GDPR, export compliance), state the risk and recommend professional review; do not give legal conclusions.

### Severity levels

| Level | Name | Meaning |
|---|---|---|
| P0 | Blocker | Near-certain rejection or upload failure. Fix before doing anything else. |
| P1 | High | Common rejection reason or serious reviewer friction; will likely bounce. |
| P2 | Medium | Risky pattern, unclear compliance, or quality concern reviewers probe. |
| P3 | Low | Polish, best practice, borderline-items — fix when convenient. |
| V | NEEDS-VERIFY | Cannot be confirmed from the repo; requires human/App Store Connect access. |

---

## Workflow

### Step 0 — Detect the stack and the mode

First determine, from the repo:

- **Stack:** native Xcode project (`*.xcodeproj`/`*.xcworkspace` + Swift/ObjC sources), React Native (`package.json` + `ios/`), Expo (`app.json`/`app.config.*`, possibly prebuild), Flutter (`pubspec.yaml` + `ios/`), or something else.
- **App categories that unlock deep-dives** (detect from code, metadata strings, dependencies, and the user's description — categories stack, e.g. a social fitness app reads two files):

  | Detected category | Read next
  |---|---|
  | any app (always) | `references/by-app-type/all-apps.md` |
  | social / UGC / chat / communities | `references/by-app-type/social-ugc.md` |
  | games (any genre, streaming, emulators) | `references/by-app-type/games.md` |
  | kids / children-primary | `references/by-app-type/kids-family.md` |
  | education / classroom | `references/by-app-type/education.md` |
  | health / fitness / medical | `references/by-app-type/health-fitness.md` |
  | finance / banking / lending / insurance | `references/by-app-type/finance-fintech.md` |
  | crypto / NFT / web3 | `references/by-app-type/crypto-web3.md` |
  | shopping / delivery / marketplace / booking | `references/by-app-type/ecommerce-marketplaces.md` |
  | streaming / music / video / books / news / reader | `references/by-app-type/media-entertainment.md` |
  | photo/video editors / creative tools | `references/by-app-type/photo-video-creative.md` |
  | dating | `references/by-app-type/dating.md` |
  | AI / generative / chatbots | `references/by-app-type/ai-apps.md` |
  | VPN / security / MDM / parental control | `references/by-app-type/vpn-privacy-security.md` |
  | gambling / betting / contests | `references/by-app-type/gambling-contests.md` |
  | business / SaaS / productivity / dev tools | `references/by-app-type/business-productivity.md` |
  | macOS / tvOS / watchOS / App Clips / extensions | `references/by-app-type/macos-platforms.md` |
  | travel / navigation / food / rideshare | `references/by-app-type/travel-food.md` |
  | lifestyle / habits / sports / weather / horoscope | `references/by-app-type/lifestyle-sports-weather.md` |
- **Mode:** one of
  - **pre-submission audit** (default) — full sweep, Steps 1–6;
  - **post-rejection triage** — the user pastes Apple's rejection message; start at Step 7;
  - **feature review** — the user asks about one feature; audit only the relevant rule files plus anything the feature touches.

### Step 1 — Inventory the risk surfaces (read `references/00-preflight.md`)

Before judging any guideline, collect the facts the guidelines are judged against:

- `Info.plist` — every permission purpose string, `UIBackgroundModes`, `NSAppTransportSecurity`, URL schemes, `LSApplicationQueriesSchemes`.
- `PrivacyInfo.xcprivacy` (own + every bundled third-party SDK's) and the required-reason APIs actually used.
- Entitlements — Sign in with Apple, push, Associated Domains, HealthKit, external purchase link entitlements, etc.
- Dependency manifests — `Podfile.lock`, `Package.resolved`, `package.json`, `pubspec.yaml`: ad SDKs, analytics, tracking SDKs, social login SDKs, chat SDKs, payment SDKs.
- Project config — bundle ID(s), targets/extensions/widgets/App Clips, min OS, `project.pbxproj` for `UIUserInterfaceStyle`, `ITSAppUsesNonExemptEncryption`.
- Source greps — `UIDevice`, identifier usage, `SFSafariViewController`, camera/mic/photos/contacts/location/HealthKit imports, StoreKit vs external checkout, `openURL` to payment pages.
- App Store Connect metadata — where a human must confirm (age rating questionnaire answers, privacy nutrition label, demo account, review notes).

Record the inventory; every later finding references it.

### Step 2–5 — Audit the five guideline sections

Work through the section files in this order (this is also roughly Apple's rejection volume order — Performance and Legal dominate, Safety produces the harshest removals, Business the most expensive fixes):

| Order | File | Contents |
|---|---|---|
| 1 | `references/02-performance.md` | completeness, demo accounts, metadata accuracy, screenshots/age rating, hardware, software requirements, private APIs, ads rules, hidden features |
| 2 | `references/05-legal.md` | privacy policy, purpose strings, data minimization, account deletion, ATT/tracking, kids/COPPA, health data, IP, gambling, VPN, MDM, code of conduct |
| 3 | `references/03-business.md` | IAP requirements, external payments, subscriptions, reader apps, crypto, gifting/tipping, loot boxes, business-model acceptability |
| 4 | `references/01-safety.md` | objectionable content, **UGC (1.2)**, kids category, physical harm, developer contact info, data security |
| 5 | `references/04-design.md` | copycats, minimum functionality, spam, extensions, Apple services, login services (4.8), Apple Pay, monetizing built-in capabilities |

For each rule: apply it only when the inventory shows it applies; record PASS, a finding (with severity + evidence + fix), or NEEDS-VERIFY. Do not paste the rule files back at the user — produce findings.

### Step 6 — Upload gate

Run the upload-validation sweep from `references/upload-errors.md`: privacy manifest completeness, deprecated APIs (UIWebView), icon/launch-screen presence, bitcode-era leftovers, missing Info.plist values. These fail before a human reviewer ever sees the build.

### Step 7 — Post-rejection triage mode

When the user brings a rejection:

1. Identify the guideline number(s) in Apple's message.
2. Look the case up in `references/rejection-cases.md` — real rejection texts and the fixes that resolved them, keyed by rule.
3. Classify the rejection type: **binary rejected** (new build required), **metadata rejected** (edit in App Store Connect, no new binary), or **information needed** (reply in Resolution Center, no resubmission yet). This determines the cheapest correct response.
4. Draft the Resolution Center reply per `references/appeals.md` — acknowledge the specific violation, describe the fix, point to where it lives in the app.
5. If the rejection looks wrong, say so and lay out the appeal path (App Review Board) — but always ship the cheap fix first when one exists; appeals are slow.

### Step 8 — Report

Emit a report in this shape:

```markdown
# App Review Audit — <app> @ <version/commit>
Mode: pre-submission | post-rejection | feature review
Stack: <detected>

## Verdict
<one paragraph: submit-ready? biggest risks; if post-rejection, cheapest path back>

## Findings
### [P0] <title> — Guideline <n>
- Evidence: <file:line / config / SDK>
- Why Apple rejects this: <one or two sentences, citing the rule's actual behavior>
- Fix: <concrete, minimal, with code/config sketch where useful>

(repeat per finding, grouped P0→P3, then NEEDS-VERIFY)

## Guideline coverage
Table: section → findings count / PASS / NEEDS-VERIFY / not applicable.

## App Store Connect checklist (human steps)
Items requiring ASC access: age rating answers, privacy label, demo account, review notes...
```

Be honest in the verdict: "no P0/P1 found, residual risk concentrated in X" is a valid outcome. Do not invent findings to seem thorough.

---

## Deep-dive triggers

Read these on top of the section files when the inventory or category detection trips them:

- **Category checklists** — after Step 0 detects the app's categories, read every matching file in `references/by-app-type/` (see the routing table above; `all-apps.md` is always read). These add category-specific rejection patterns, audit specifics, and pre-submission checklists the section files don't carry.
- **Any rejection message from Apple** → `references/rejection-cases.md`, then `references/appeals.md`.
- **Upload failure / email from App Store Connect with "ITMS-"** → `references/upload-errors.md`.

## What this skill is not

- Not a substitute for testing: crashes and broken flows (2.1) are the #1 rejection family and only a real device run proves them.
- Not a lawyer, not Apple. Flag regulatory findings; recommend counsel where the guidelines themselves say "check with a lawyer."
- Not frozen: Apple updates the guidelines several times a year and App Review applies them with judgment ("we'll know it when we see it"). Re-run this audit before every submission.
