# 5. Legal — Guideline Section 5

Legal is the #2 rejection family by volume (privacy alone dwarfs most others) and the section most entangled with upload-time automated checks. The Oct 2026 revision also hardened the section intro: apps facilitating human trafficking or exploitation of children are removed and blocked from alternative distribution, with authorities notified.

## 5.1 Privacy

### 5.1.1 Data Collection and Storage

| Rule | Requirement | Audit evidence |
|---|---|---|
| (i) Privacy policy | link in **App Store Connect metadata AND in-app, easily accessible**; policy must say what data is collected and how, guarantee third parties (analytics, ad networks, SDKs, related entities) give equal protection, and explain retention/deletion + how to revoke consent and request deletion | in-app settings link present? URL live? policy text covers SDKs actually present? |
| (ii) Permission & consent | consent for collection even of "anonymous" data; paid features must not depend on granting data access; easy way to withdraw consent; **purpose strings must clearly and completely describe use**; GDPR legitimate-interest route must comply with all terms | purpose-string text review; paywalls gated on tracking permission; consent screens |
| (iii) Data minimization | request only data relevant to core functionality; prefer out-of-process pickers/share sheets over full Photos/Contacts access | `PHPhotoLibrary` full-access vs `PHPickerViewController`; Contacts full fetch vs `CNContactPickerViewController` |
| (iv) Access | respect permission settings; no manipulating/tricking/forcing consent; unrelated permission bundling forbidden (photo posting must not require mic); offer alternatives when consent denied (manual address entry when location declined) | code paths after permission denial — does the app still work? |
| (v) Account Sign-In | no login without significant account-based features; **account creation ⇒ in-app account deletion**; no personal info required except when core to functionality or required by law; if core functionality isn't a specific social network (Facebook, WeChat, Weibo, X…), don't force that login — basic profile/sharing/invites are not "core"; must include a mechanism to **revoke social credentials** and disable data access from within the app; social tokens must stay on-device and only be used to connect directly to that network while in use | login gate analysis; `delete account` flow (initiation in-app, not just a link to a website that makes you hunt); token storage locations |
| (vi) | no surreptitious password/private-data discovery | password fields, keylogging patterns |
| (vii) | `SFSafariViewController` visible, never hidden/obscured; no tracking through it | overlay views above SFSVC |
| (viii) | no compiling personal info from non-user sources, even public databases | scraped-profile features |
| (ix) | regulated fields (banking, health, gambling, cannabis, air travel, crypto exchanges) submitted by a **legal entity** providing the service, not individuals; cannabis geo-restricted to legal jurisdictions | developer account type vs app category |
| (x) | basic contact info (name/email) request must be optional, not condition features/services | gate analysis |

**Account deletion details (the biggest 2022+ practical addition):** the flow must be discoverable, let users initiate deletion **inside the app**, and not be a maze (placing only a URL to a hard-to-find web form has been rejected when the app supports account creation). Temporary deactivation ≠ deletion. If deletion must be restricted for legal reasons (regulated finance), explain in review notes.

### 5.1.2 Data Use and Sharing

- (i) No use/transmission/sharing of personal data without permission; disclose where data goes, **including to third-party AI**; explicit permission before sharing with third parties; **tracking requires ATT authorization** (`ATTrackingManager.requestTrackingAuthorization` before any `advertisingIdentifier` access / ad SDK init); functionality may not be gated on enabling system features (push, location, tracking) — including "compensation" like gift cards for enabling tracking; violations can mean removal from sale + Program removal.
- (ii) No repurposing data collected for one purpose without new consent.
- (iii) No surreptitious user profiling; no re-identification of "anonymized" data; no encouraging others to de-anonymize.
- (iv) No contact databases built from Contacts/Photos for your own use or sale; no collecting installed-app lists for analytics/ads.
- (v) No contacting people from a user's Contacts/Photos except at that user's explicit individual initiative — **no Select-All**, no default-selected contacts; show the user how the message will appear (content + sender) before sending.
- (vi) HomeKit/HealthKit/Clinical Records/MovementDisorder/ClassKit/depth-and-face-mapping data never used for marketing/ads/use-based data mining.
- (vii) Apple Pay data shared only to facilitate/improve delivery of goods/services.

**Evidence:** ATT presence × ad/analytics SDKs; SDK init order (before/after authorization); `LSApplicationQueriesSchemes` app-detection; invite-friend flows (select-all patterns); any AI API calls carrying user text/images.

### 5.1.3 Health and Health Research

(i) Health data never used for ads/marketing/mining except for the user's direct benefit (submitted by the benefiting entity, no third-party sharing); disclose specific health data collected. (ii) No false data into HealthKit; no personal health info in iCloud. (iii) Human-subject research: full informed consent (nature/purpose/duration, procedures/risks/benefits, confidentiality & sharing, contact point, withdrawal process). (iv) Ethics-review-board approval, provable on request.

### 5.1.4 Kids

(a) COPPA/GDPR-grade care; birthdate/parental contact requested only for statutory compliance; app must still be useful regardless of age; apps **primarily for kids**: no third-party analytics or ads. (b) Limited exceptions per 1.3's strict terms; privacy policy + children's-privacy-statute compliance required for any app collecting personal info from minors (chat ability, photos, persistent identifiers + personal data); **parental gate ≠ parental consent** — both may be needed; "For Kids/Children" wording in metadata is reserved for the Kids Category.

### 5.1.5 Location

Only when directly relevant to features; never for emergency services or autonomous control of vehicles/aircraft (small drones/toys/car alarms excepted); notify + consent before collecting/transmitting/using location; explain purpose in-app.

## 5.2 Intellectual Property

| Rule | Requirement |
|---|---|
| 5.2.1 | only content you created or licensed; no misleading/copycat names or metadata; submit as the rights owner/licensee |
| 5.2.2 | using/monetizing/displaying third-party service content requires permission under that service's ToS — authorization shown on request |
| 5.2.3 | **no saving/converting/downloading media from Apple Music, YouTube, SoundCloud, Vimeo etc. without authorization**; even streaming can violate ToS |
| 5.2.4 | never imply Apple endorses/sources your app; "Editor's Choice" badge is applied by Apple only |
| 5.2.5 | don't look confusingly like an Apple product/interface (Finder, App Store, Messages); no Apple emoji in apps/keyboards/stickers; iTunes/Apple Music previews not for entertainment value (game soundtracks, collage BGM) — and previews must link to the store item; Activity rings only per HIG; WeatherKit data with attribution |

**Evidence:** asset provenance (stock licenses, scraped images), brand mentions in copy, third-party API usage without documented authorization, downloader functionality, emoji assets in bundle.

## 5.3 Gaming, Gambling, Lotteries

- 5.3.1 sweepstakes/contests sponsored by the developer.
- 5.3.2 official rules in-app, stating Apple is not a sponsor.
- 5.3.3 **never** use IAP to buy credit/currency for real-money gaming.
- 5.3.4 real-money gaming: necessary licensing, **geo-restricted** to those locations, app must be **free**; illegal gambling aids (card counters) banned; lotteries need consideration/chance/prize.

## 5.4 VPN Apps

Must use `NEVPNManager`; **organization accounts only**; clear on-screen declaration of collected data and its uses **before** any purchase/use; privacy policy commits to never selling/using/disclosing data to third parties; local-law compliance, license info in Review Notes where required. Violations = removal + alternative-distribution block + possible Program removal.

## 5.5 Mobile Device Management

MDM capability requested from Apple; only commercial enterprises/education/government (limited: parental control/device security companies); pre-use data declaration; no data sale/disclosure; limited third-party analytics (app performance only, never user/device/app data); configuration-profile apps included.

## 5.6 Developer Code of Conduct

Respect in reviews/support/ASC communications — harassment, manipulation, or fraud ends in Program termination. No preying on users, tricky pricing, charging for undelivered features, manipulative practices in or out of the app.

- **5.6.1** respond to App Store reviews respectfully, no personal info/spam/marketing in responses; use `SKStoreReviewController` for prompts — **custom review prompts are disallowed**.
- **5.6.2** truthful, current developer identity on the store.
- **5.6.3** no discovery fraud: manipulating charts, search, reviews, referrals; fake/incentivized/filtered reviews.
- **5.6.4** quality expectations: excessive negative reviews/refund requests can itself be a Code-of-Conduct factor.

## Cross-cutting: export compliance

Apps using encryption must answer ASC's export-compliance questions; set `ITSAppUsesNonExemptEncryption` where applicable. "Missing export compliance declaration" is a common upload-time hold. France-only extra declarations for certain crypto; standard exempt: only Apple-provided crypto (HTTPS via URLSession etc.).

## Audit heuristics for this section

1. Purpose-string table: every `NS*UsageDescription` × every framework actually used × text quality (specific, app-named, benefit-led). Vague strings ("Camera access needed") are a top-3 rejection.
2. Privacy manifest × required-reason APIs × privacy nutrition label: three-way consistency (see `upload-errors.md` for the mechanical checks).
3. Login map: forced social login? token storage? revocation? deletion?
4. Tracking map: ATT × SDK init order × nutrition label "Tracking" answer.
5. Contact/invite flows: select-all, pre-selected recipients, message previews.
6. Rights inventory for every non-original asset and every third-party data source.
