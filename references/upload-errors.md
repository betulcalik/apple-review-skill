# Upload Validation Errors (ITMS-*) — the gate before App Review

These emails arrive from App Store Connect *before a human reviewer sees the build* ("Dear Developer, we identified one or more issues with a recent delivery request…"). They are deterministic: find the code in the email, apply the fix, re-upload. A submission stuck here never enters review. Codes evolve; when the email text doesn't match a row below, fix by symptom.

## The frequent ones

| Code | Symptom / email shape | Fix |
|---|---|---|
| **ITMS-91053** | "Missing API declaration — Privacy. Your app uses … APIs that require you to declare usage in a privacy manifest" | Add/extend `PrivacyInfo.xcprivacy` with `NSPrivacyAccessedAPITypes` for every required-reason API. The 2024+ categories: **UserDefaults (CA92.1)** · **file timestamp APIs (C617.1 / 3B52.1)** · **system boot time (35F9.1 / 8FFB.1)** · **disk space (E174.1 / 85F4.1)** · **active keyboard (3EC5.1)**. Also triggered by *bundled third-party SDKs* lacking their own manifests — update the SDK, or (last resort) vendor a manifest for it |
| **ITMS-91054** | "Your privacy manifest is invalid" — malformed plist, bad reason codes, wrong keys | Validate the plist structure; reason codes must match Apple's approved list exactly |
| **ITMS-90809** | "Deprecated API Usage — Apple will stop accepting submissions of apps that use UIWebView APIs" | Remove every `UIWebView` reference (search source **and** compiled binaries of vendored SDKs — grep the SDK binary: `strings SDK.a | grep UIWebView`). WKWebView is the replacement |
| **ITMS-90683** | "Missing Purpose String — … does not include the NSPhotoLibraryUsageDescription / NSCameraUsageDescription / … key" | Add the specific `NS*UsageDescription` key the email names. Always add purpose strings when the linked framework requires them, even if you think the feature is dormant |
| **ITMS-90713** | "Missing Info.plist value — CFBundleIconName" | Asset catalog icon set referenced properly; icon set includes all required sizes incl. single 1024×1024 marketing icon (no alpha channel) |
| **ITMS-90035** | "Invalid Signature — make sure you have signed your application with a distribution certificate" | Clean rebuild; correct signing identity (Apple Distribution, not Mac Development); verify embedded frameworks are signed; check for stray `__MACOSX` artifacts |
| **ITMS-90426** | "Invalid Swift Support — Swift libraries found in the wrong location / files not embedded" | Rebuild/archive with current Xcode; don't hand-modify the IPA; check `Frameworks/` layout and that Swift runtime libs are embedded or min OS handles them |
| **ITMS-90078** | "Missing required icon file" (family: icon asset errors — also "invalid app store icon", alpha-channel complaints) | Complete icon set in the asset catalog; marketing icon 1024×1024, **no alpha**; correct `CFBundleIcons` payload |
| Beta/old toolchain | "built with a beta version of Xcode" / "SDK version no longer accepted" | Rebuild with the latest shipping Xcode; after WWDC, betas are upload-only anyway — but don't ship App Store builds from betas past their welcome |

## Symptom families (match by text when the code differs)

- **Privacy-manifest family (91053/91054):** anything about "privacy manifest", "required reason API", "NSPrivacyAccessedAPITypes". The complete fix: own manifest + every SDK's manifest + nutrition label consistency (see `05-legal.md` §5.1).
- **Purpose-string family (90683):** names the exact key. Keys Apple commonly demands: `NSCameraUsageDescription`, `NSPhotoLibraryUsageDescription`, `NSPhotoLibraryAddUsageDescription`, `NSMicrophoneUsageDescription`, `NSLocationWhenInUseUsageDescription`, `NSLocationAlwaysAndWhenInUseUsageDescription`, `NSContactsUsageDescription`, `NSHealthShareUsageDescription`, `NSHealthUpdateUsageDescription`, `NSMotionUsageDescription`, `NSBluetoothAlwaysUsageDescription`, `NSLocalNetworkUsageDescription`, `NSFaceIDUsageDescription`, `NSUserTrackingUsageDescription`, `NSAppleMusicUsageDescription`, `NSCalendarsUsageDescription`, `NSRemindersUsageDescription`, `NSSpeechRecognitionUsageDescription`, `NSHomeKitUsageDescription`, `NSNearbyInteractionUsageDescription`.
- **Icon/asset family (90078/90713 and friends):** icon set completeness, alpha channel, single-size vs full set, Apple Watch icons, marketing icon.
- **Signing family (90035 and friends):** distribution cert, embedded framework signing, entitlements/profile mismatch (profile must include the entitlements the binary requests — e.g. SIWA, push, external-purchase-link).
- **Structure family (90426 and friends):** framework embedding, Swift libs, extensions' launch paths, `Frameworks/` in extensions (extensions must not embed Swift libs the app already embeds).
- **Deprecated-API family (90809):** UIWebView historically; anything else Apple announces as deprecated-blocked in release notes — keep `UIWebView`, old `codec`/`AudioSession`-era APIs out of vendored binaries.

## Pre-upload sweep (run before every upload)

1. Every required-reason API used (own code + SDKs) has a matching manifest entry — including "innocent" ones: `UserDefaults` is the #1 surprise trigger.
2. Every linked privacy-triggering framework has its purpose string.
3. Icon set complete; marketing icon has no alpha.
4. No `UIWebView` anywhere, including vendored `.a`/`.framework` binaries (grep with `strings`/`nm`).
5. Build with latest shipping Xcode; no leftover beta toolchain.
6. `ITSAppUsesNonExemptEncryption` set (or plan to answer ASC's export question).
7. Entitlements ⊆ provisioning profile; extension targets' IDs nested under the app's.

If an ITMS email already arrived: the email lists every offending file path — treat it as a to-do list, fix all of them in one upload (partial fixes bounce again).
