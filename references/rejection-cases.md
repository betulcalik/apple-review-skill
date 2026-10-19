# Rejection Case Database — keyed by guideline

Real-world rejection patterns compiled from documented App Review outcomes (developer reports, rejection-letter archives, forum/Stack Overflow cases). Use for post-rejection triage: match the user's message, classify the response type, apply the documented fix.

**Response types (determines the cheapest correct action):**

- **BIN** — binary rejected: fix requires a new build upload.
- **META** — metadata rejected: edit in App Store Connect only; no new binary, no re-review of code.
- **INFO** — information needed: reply in Resolution Center; app stays in review state; no resubmission until Apple is satisfied.
- **REMOVAL** — app pulled / account action: Code-of-Conduct territory; follow `appeals.md` carefully.

---

## Guideline 1 — Safety

| Case | Type | Apple's actual message (shape) | Documented fix |
|---|---|---|---|
| 1.2 missing UGC features | BIN | "Your app enables the display of user-generated content but does not include the required features to moderate this content…" then lists which of the four (filter/report/block/contact) are missing | implement the missing items exactly; list locations in review notes; turnaround is fast |
| 1.2 app used primarily for objectionable content | REMOVAL | "…your app is being used primarily for objectionable content, including [porn/anonymous bullying/objectification]. Apps that are used primarily for such purposes do not belong on the App Store and may be removed without notice" | structural change: verified identity, real-time NSFW detection, remove anonymous/random matching; moderation logs as appeal evidence |
| 1.2.1 creator platform missing age restriction | BIN | creator app lacks a way to identify over-rating content / no verified-or-declared-age gate | age gate + per-content rating flags |
| 1.1.4 sexual content | BIN | explicit material or hookup-app usage patterns | remove content/feature; age rating honesty; moderation for UGC sources |
| 1.1.6 false features | BIN | fake tracker/prank functionality; "entertainment purposes" disclaimer rejected | remove the deceptive function |
| 1.1 objectionable content in marketing | META | Apple crawled the product website and found objectionable material | clean marketing pages referenced in metadata |
| 1.3 kids missing parental gate | BIN | links out / purchases not behind parental gate | add gate; move features behind it |
| 1.3 kids third-party SDK | BIN | analytics/ads SDK in kids app | remove SDK or switch to Kids-compliant provider (no IDFA, no child identifiers) |
| 1.4.1 medical accuracy | INFO | "provide data and methodology to support accuracy claims" | disclose methodology; add doctor-consult reminders; supply regulatory docs; remove unsupported sensor claims (BP/SpO2/temp) |
| 1.4.2 dosage calculator entity | BIN | submitter is not an approved entity | ship from manufacturer/hospital/university/pharmacy or get FDA-equivalent approval |
| 1.4.3 substances | BIN | tobacco/vape/drug promotion or sales facilitation | remove promotion; geo-restrict licensed cannabis |
| 1.5 contact info | BIN/META | support URL missing/dead or no in-app contact | fix URL; add in-app support screen |
| 1.6 data security | BIN | plaintext credentials/PII, insecure storage/secrets in binary | remove secrets, encrypt transport & at-rest storage |

## Guideline 2 — Performance

| Case | Type | Message shape | Documented fix |
|---|---|---|---|
| 2.1 crashes during review | BIN | crash log attached, "specifically, your app crashed on launch/when we tapped X" | reproduce from the log; fix; note testing in review notes |
| 2.1 demo account not working | INFO | "We were unable to sign in with the demo account provided" | permanent account, bypass OTP, excluded from rate limits; verify before every submission |
| 2.1 placeholder content | BIN | "placeholder text, empty websites, and other temporary content" — Lorem ipsum/coming soon/empty lists | finish screens; seed content; remove test fixtures |
| 2.1 missing icon/launch screen | BIN (upload) | build rejected for missing asset catalog entries | complete asset catalog |
| 2.1 IAP products not found | BIN/INFO | configured IAPs not discoverable in binary | product states "Ready to Submit", attached to the version, IDs match, sandbox works |
| 2.1(b) IAP not submitted with binary | BIN | products in "Missing Metadata" / not selected in the submission | complete IAP metadata + include in version submission |
| 2.1 backend down | INFO | features fail during review | staging up during review window; document environment in notes |
| 2.1 external hardware not provided | INFO | reviewer can't test hardware-dependent flow | demo video + hardware loaner program, or demo mode |
| 2.1 requires external hardware / account we can't access | INFO | login requires code only developer receives | pre-verified demo account |
| 2.2 beta quality | BIN | app presents as demo/beta/concept | finish the app; remove beta branding |
| 2.3.1(a) insufficient review notes | INFO | "generic descriptions will be rejected" — new features not specifically described | write per-feature descriptions + where to find them |
| 2.3.1(a) review-env detection | REMOVAL | remote config changed behavior for reviewers | never ship behavior keyed to review environment |
| 2.3.2 IAP not indicated in metadata | META | description/screenshots don't say what requires purchase | add "subscription/purchase required for X" disclosures |
| 2.3.3 screenshots not app-in-use | META | title art/login page/splash only | re-capture real screens per device class |
| 2.3.3 iPhone screenshots for iPad | META | wrong device screenshots | per-device screenshots (Fastlane snapshot helps) |
| 2.3.6 age rating wrong | META | questionnaire answers don't match content | answer honestly; adjust rating & content policy together |
| 2.3.7 name/keywords | META | trademark terms, other app names, prices, keyword stuffing, name > 30 chars | clean metadata; unique name |
| 2.3.7 misleading description | META/META+ | app doesn't do what description promises | align marketing to reality (virus-scan style claims are fatal) |
| 2.3.8 mature content in screenshots | META | metadata must be 4+-appropriate | choose neutral frames |
| 2.3.10 other platforms referenced | META | Android/other-marketplace names, icons, imagery in app or metadata | remove cross-platform references |
| 2.3.12 What's New generic for major changes | META | significant changes described only as "bug fixes and improvements" | enumerate real changes |
| 2.4.1 broken on iPad | BIN | layout broken / unresponsive on iPad | fix layouts or remove iPad from device list (last resort) |
| 2.4.2 battery/resource abuse | BIN | excessive drain; background mining | fix background work; remove mining |
| 2.5.1 private APIs | BIN (upload or review) | "use of non-public API" (often caught at upload) | replace with public API |
| 2.5.2 hot-patch/code download | BIN | app downloads and executes code changing features | remove JSPatch/CodePush-style feature changes; OTA for fixes only where permitted |
| 2.5.4 unused background modes | BIN (upload) | declared mode without purpose | remove `UIBackgroundModes` entries |
| 2.5.5 IPv6 failure | BIN | fails on IPv6-only network | remove IPv4/IPv6 assumptions; test NAT64 |
| 2.5.9 overridden system behavior | BIN | altered switch/UI behavior, blocked expected links | restore standard behaviors |
| 2.5.14 recording without indicator | BIN | recording without clear consent/indicator | add visible/audible indicator + consent |
| 2.5.18 ads in widget/extension/Clip | BIN | display advertising outside main binary | remove ads from extension surfaces |

## Guideline 3 — Business

| Case | Type | Message shape | Documented fix |
|---|---|---|---|
| 3.1.1 external payment for digital goods | BIN | Stripe/PayPal/web checkout for digital unlock | move to StoreKit IAP |
| 3.1.1 license key/QR/AR unlock | BIN | non-IAP unlock mechanism | convert to IAP |
| 3.1.1 missing Restore Purchases | BIN | restorable purchases without a restore path | add visible Restore button + `AppStore.sync()`/restore handler |
| 3.1.1 expiring currency | BIN | purchased currency expires | remove expiry |
| 3.1.1 loot boxes without odds | BIN | randomized items purchased without odds display | show odds before purchase |
| 3.1.1 NFT ownership unlocking features | BIN | external NFT state gates app features | decouple features from NFT ownership |
| 3.1.1(a) external link w/o entitlement (non-US) | BIN | purchase link outside US storefront without entitlement | remove link or apply for entitlement + consent sheet |
| 3.1.2 missing subscription info | BIN | paywall lacks title/length/price disclosures | full 3.1.2(c) disclosures + EULA/privacy links |
| 3.1.2 misleading pricing presentation | BIN | calculated price ("$0.33/day") more prominent than billed price | billed terms equal-or-more prominent |
| 3.1.2 free-trial-as-free-app | BIN | "Start Free Trial" implies free app | label trial, show post-trial price before start |
| 3.1.2 intro price mislabeled as trial | BIN | paid intro period presented as free trial | label as discounted period with real price |
| 3.1.2(a) no ongoing value | BIN | one-time content behind a subscription | deliver recurring value or switch to non-consumable |
| 3.1.2(a) features removed from paid users | BIN | business-model switch stripped existing buyers | grandfather purchased functionality |
| 3.1.2(b) multiple simultaneous subs | BIN | user can stack tiers of the same service | implement upgrade/downgrade crossgrades |
| 3.1.2(c) vague offer | BIN | "Premium" without saying what's included | itemize what the subscription includes |
| 3.1.3(a) fake reader app | BIN | reader exception claimed by non-reader app | reclassify; use IAP |
| 3.1.3(d) group classes as p2p | BIN | one-to-many service claiming person-to-person | IAP for group offerings |
| 3.1.5 crypto licensing | INFO | exchange without regional license | restrict storefronts; provide license in notes |
| 3.1.5 wallet from individual account | BIN | wallets require organization enrollment | organization account |
| 3.1.5 on-device mining | BIN | mining in app or via ads | remove |
| 3.2.1(vii) tipping with a cut | BIN | platform takes % of "gift" | 100% pass-through or use IAP |
| 3.2.2(i) app storefront for other apps | BIN | interface for installing third-party apps | remove store-like collection UI |
| 3.2.2(iii) ad-saturated app | BIN | predominantly ads | add substance; reduce ad density |
| 3.2.2(ix) loan terms | BIN | missing APR disclosure / APR > 36% / ≤60-day repayment | full disclosure + legal terms (or don't ship) |
| 3.2.2(x) forced rating gate | BIN | functionality gated on rating/review/download | remove gate; use `SKStoreReviewController` |
| 3.0 irrationally high price | BIN | price far beyond comparable value | adjust price |

## Guideline 4 — Design

| Case | Type | Message shape | Documented fix |
|---|---|---|---|
| 4.2 WebView wrapper | BIN | "repackaged website", limited native functionality | native navigation/features or withdraw |
| 4.2 minimal functionality | BIN | single screen, limited content/interaction | add real functionality |
| 4.2.3(ii) undisclosed download | BIN | large initial download without size disclosure/prompt | disclose + prompt before download |
| 4.2.6 template app by agency | BIN | commercialized template submitted for client | provider submits; customization tools; or picker-model single binary |
| 4.3 duplicate apps | BIN/REMOVAL | same app across Bundle IDs / similar binaries | consolidate to one app; vary genuinely |
| 4.3(b) saturated category | BIN | "another dating/flashlight/wallpaper/timer/fortune app without meaningful differences" | demonstrate meaningful differentiation |
| 4.4.1 keyboard full access | BIN | keyboard demands full access/network unnecessarily | functional without full access |
| 4.4.2 Safari extension broad permissions | BIN | access to more sites than necessary | minimal host permissions |
| 4.5.4 push marketing w/o opt-in | BIN | promotional push without consent/opt-out | consent language + opt-out |
| 4.5.4 push required | BIN | app unusable without enabling push | degrade gracefully |
| 4.8 missing SIWA | BIN | social login without equivalent Apple option | add SIWA at equal prominence |
| 4.8 SIWA not equivalent | BIN | hidden/de-emphasized SIWA | same screen, comparable visual weight |
| 4.9 Apple Pay disclosures | BIN | recurring charge disclosures missing | term/what/charges/cancel disclosures |
| 4.10 monetizing built-ins | BIN | charging for OS capabilities | unbundle; charge for your service instead |
| 4.0 broken layout / unreadable / tiny targets | BIN | quality/design rejection citing design section | fix layouts, Dynamic Type, contrast, touch targets |

## Guideline 5 — Legal

| Case | Type | Message shape | Documented fix |
|---|---|---|---|
| 5.1.1(i) no privacy policy | BIN/META | policy link missing in ASC or app | publish policy; link both places |
| 5.1.1(ii) purpose strings weak/missing | BIN | "purpose strings… do not clearly explain" or ITMS-90683 at upload | specific, app-named, benefit-led strings |
| 5.1.1(iii) unnecessary permissions | BIN | permission not tied to core functionality | remove request; use pickers |
| 5.1.1(iv) permission manipulation | BIN | pre-permission screens that pressure/delay | request directly at need; work when denied |
| 5.1.1(v) no account deletion | BIN | account creation without in-app deletion | full deletion flow initiated in-app |
| 5.1.1(v) login required w/o account features | BIN | forced login for non-account app | allow app use without login |
| 5.1.1(ix) individual dev in regulated field | BIN | banking/health/gambling/crypto from individual account | legal-entity account |
| 5.1.1 privacy manifest missing/mismatch | BIN (upload, ITMS-91053) | required-reason APIs undeclared | add/complete `PrivacyInfo.xcprivacy` |
| 5.1.1 label mismatch | INFO | nutrition label disagrees with actual collection | align label, SDKs, manifest |
| 5.1.2(i) tracking w/o ATT | BIN | ad SDK reads IDFA before authorization | ATT gate before SDK init |
| 5.1.2(i) gating on permissions | BIN | features gated on push/location/tracking enable | remove gates; alternative paths |
| 5.1.2(vi) HealthKit for ads | BIN | health data used for targeting | remove from ad pipeline |
| 5.1.4 "for kids" wording | META | kids-implying metadata outside Kids Category | reword metadata |
| 5.2.1 copyrighted material | BIN | assets/content without rights | license/remove |
| 5.2.2 unauthorized API use | INFO | third-party service content w/o ToS permission | API agreement or remove |
| 5.2.3 media downloading | BIN | saving/converting third-party media | remove downloader |
| 5.2.5 Apple-confusable design | BIN | looks like App Store/Finder/Messages; Apple emoji in binary | redesign; replace assets |
| 5.2 export compliance | upload hold | missing encryption declaration | answer ASC questions; `ITSAppUsesNonExemptEncryption` |
| 5.3.4 gambling w/o license | BIN | real-money gaming without geo/license/free requirements | license + geo-restriction + free app |
| 5.4 VPN individual account | BIN | VPN from individual developer | organization enrollment |
| 5.4 VPN data declaration | BIN | missing pre-use data declaration screen | add declaration before purchase/use |
| 5.6 quality / not eligible for resubmission | REMOVAL | Code of Conduct quality finding | address root cause; written improvement plan for account restoration |
| 5.6.3 review manipulation | REMOVAL | incentivized/fake reviews, chart manipulation | stop; remove incentivization; respond honestly |

---

## Matching procedure (post-rejection triage)

1. Extract the guideline number from Apple's message (format: `Guideline X.Y - Section - Subtitle`).
2. Look up rows above; pick the one whose quoted symptoms match the specifics in the message (Apple usually pastes the specific missing item).
3. Classify BIN vs META vs INFO: the email's "Next Steps" say whether a new binary is needed ("…resubmit your binary" vs "edit your metadata in App Store Connect" vs "reply in Resolution Center").
4. Apply the documented fix; if unclear, open the rule file `references/0X-*.md` for the full requirement list.
5. Draft the reply per `references/appeals.md`.
