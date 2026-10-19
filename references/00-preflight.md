# 0. Preflight — Discovery & "Before You Submit"

Two things live here: (A) how to find the facts every later rule is judged against, and (B) Apple's own "Before You Submit" list — the items App Review checks before anything else.

## A. Project discovery — what to read, in what order

Do this inventory **before** applying any guideline. Record everything; later findings cite it.

### Native Xcode project

| Artifact | What to extract |
|---|---|
| `*.xcodeproj/project.pbxproj` | all targets (app, extensions, widgets, watch, TV, clips), bundle IDs, deployment targets, `INFOPLIST_KEY_*`, capabilities, `UIUserInterfaceStyle`, `ITSAppUsesNonExemptEncryption` |
| `Info.plist` (per target) | every `NS*UsageDescription` purpose string (exact text), `UIBackgroundModes`, `NSAppTransportSecurity` exceptions, `NSUserTrackingUsageDescription`, URL schemes, `LSApplicationQueriesSchemes`, `CFBundleDisplayName` vs ASC name |
| `*.entitlements` | aps-environment, Sign in with Apple, Associated Domains, HealthKit, HomeKit, Wallet, external-purchase-link entitlements, App Groups, iCloud container overrides |
| `PrivacyInfo.xcprivacy` (own + bundled SDK copies) | NSPrivacyAccessedAPITypes + reasons, NSPrivacyCollectedDataTypes, NSPrivacyTracking, NSPrivacyTrackingDomains |
| `Podfile.lock` / `Package.resolved` | every SDK: analytics, ads, crash, social login, chat, payments, attribution |
| Source greps | see Grep battery below |
| Assets | AppIcon completeness (all sizes incl. 1024 marketing), launch screen (storyboard vs info values) |

### React Native / Expo

- `package.json` + lockfile → ad/analytics/tracking/social SDKs (`react-native-tracking-confusion` style audits: `react-native-fbsdk-next`, `react-native-google-signin`, `react-native-branch`, `react-native-idfa-aaid`, `@react-native-firebase/analytics`, `react-native-adjust`...).
- Expo: `app.json` / `app.config.*` → `ios.infoPlist` purpose strings, plugins (`expo-tracking-transparency`, `expo-notifications`), `ios.privacyManifests` (SDK 51+), EAS config for entitlements.
- `ios/` folder (prebuilt) → same native checks as above.
- OTA updates: `expo-updates`/CodePush config — major feature changes via OTA violate 2.5.2/3.3.1-era rules; bug fixes only.

### Flutter

- `pubspec.yaml` → same SDK categories; `ios/Runner/Info.plist` purpose strings; `flutter` plugins that link native SDKs.

### Grep battery (all stacks)

```bash
# Identifiers / tracking
rg -n "identifierForVendor|advertisingIdentifier|ASIdentifierManager" --type swift
# ATT
rg -n "ATTrackingManager|requestTrackingAuthorization"
# Full-access permission contexts
rg -n "PHPhotoLibrary|CNContactStore|CLLocationManager|requestAlwaysAuthorization|HKHealthStore"
# Camera / mic / recording
rg -n "AVCaptureSession|SCCameraKit|ReplayKit|AVAudioRecorder"
# Payments — the dangerous ones
rg -n "Stripe|Checkout\\.com|Paddle|PayPal|RevenueCat|StoreKit|SKProduct|Product.purchase"
rg -n "openURL|Linking.openURL" | rg -i "pay|checkout|subscribe|upgrade|buy|pricing|web\\.app"
# Web views
rg -n "WKWebView|SFSafariViewController|WebView"
# Dynamic code / hot patch
rg -n "JSPatch|eval\(|JavaScriptCore|dlopen| NSTask"
# Private API smells
rg -n "NSSelectorFromString|_\\w+(Alpha|Beta|Recorder)|performSelector" --type swift
# Social login
rg -n "Facebook|GoogleSignIn|SignInWithApple|ASAuthorizationAppleIDButton|Login with|WeChat|Twitter/X"
# Account deletion
rg -ni "delete.*account|account.*delet" 
# Hardcoded secrets (also 5.1.1 risk)
rg -n "sk_live|api[_-]?key\\s*=\\s*['\\\"][A-Za-z0-9]{16,}" -g '!*.md'
```

Anything the greps surface goes into the inventory with file:line. Missing results are evidence too (e.g. no `delete account` hits + account creation present ⇒ 5.1.1(v) finding).

### App Store Connect (human-only — mark NEEDS-VERIFY)

Age-rating questionnaire answers, privacy nutrition label, category selection, screenshots/previews per device, demo account credentials in review notes, `What's New` text, copyright field, export compliance answer, Content Rights answers, sign-in information completeness. Ask the user for screenshots/export of these when mode is pre-submission.

## B. Apple's "Before You Submit" list — mapped

Apple publishes this as the pre-checklist before the guidelines even start. Every item below has ended real submissions:

| Item | Rule it feeds | How to verify here |
|---|---|---|
| Tested on-device for crashes/bugs | 2.1 | ask user; check for TODO/FIXME/`#if DEBUG` gated main flows, stub'd services, force-unwraps in launch path |
| Metadata complete & accurate | 2.3 | NEEDS-VERIFY in ASC; check name/keywords in repo configs for stuffing, prices, trademarks |
| Contact info current | 1.5 | support URL reachable? in-app contact present? |
| Full access for review: demo account or fully-featured demo mode, hardware/resources (QR code, etc.) | 2.1 | demo account present in plan? backend live during review window? |
| Backend services live and accessible during review | 2.1 | staging gate? auth provider test users? feature flags defaulting off for review (dangerous — see 2.3.1(a) review-env detection) |
| Detailed review notes for non-obvious features & IAP | 2.3.1(a), 2.1(b) | offer to draft them from the audit |
| Regulatory documentation where required | 5.x | health (1.4.1), gambling (5.3.4), crypto (3.1.5), banking (5.1.1(ix)), nonprofit status, HIPAA/federal approval letters |

### Demo-account failure patterns (top practical killer)

App Review rejects under 2.1 "Information Needed" when:

- demo account not provided at all while login exists;
- demo credentials expired, rate-limited, or behind SMS/email OTP the reviewer can't receive — provide a bypass or a pre-verified demo account;
- staging reset wiped the account between submissions;
- backend/feature-flag down during review (review happens in US business hours typically);
- reviewer needs hardware (NFC toy, OBD dongle, printer) or a specific environment (specific city, specific account role) not documented;
- app region-locks sign-up but availability includes other regions;
- login requires invite codes without providing one.

Fix pattern: permanent dedicated demo account, excluded from rate limits and OTP, documented in review notes with the exact screens it unlocks, plus a video walkthrough link for hardware-dependent flows.
