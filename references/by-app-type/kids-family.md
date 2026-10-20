# Kids & Family — App-Type Checklist

**Covers:** Kids Category (opt-in), apps primarily for children, family/parenting apps touching child data.

## The Kids Category is a different regime

Once opted in, expectations stick **even if you later deselect the category** — Apple keeps applying Kids rules if users expect them.

Hard requirements:

- **No third-party analytics, no third-party advertising** (1.3, 5.1.4(a)). Limited exceptions: analytics that never collect/transmit IDFA or anything identifying a child (name, DOB, email, location, device/network info); contextual ads only from providers with documented Kids practices + human review of creatives (1.3, 5.1.4(b)).
- **No links out of the app, purchasing opportunities, or distractions** unless behind a designated **parental gate** (1.3) — math challenge / hold-to-continue patterns are the accepted implementations.
- No PII or device information sent to third parties (1.3).
- COPPA (US), GDPR-K / age-appropriate-design style statutes (EU and elsewhere) compliance (5.1.4); birthdate + parental contact requested only for statutory compliance; app still useful regardless of age answers.
- **Parental gate ≠ parental consent** (5.1.4(b)) — a gate for links ≠ consent to collect data; both may be needed.

## Top rejection reasons

| Risk | Guideline |
|---|---|
| Kids app ships Firebase/AdMob/Adjust/Facebook SDK (or any IDFA touch) | 1.3, 5.1.4 — near-automatic |
| External link / "more apps" / social button without parental gate | 1.3 |
| "For Kids"/"For Children" wording in metadata while not in Kids Category (or vice versa expectations) | 2.3.8, 5.1.4(b) |
| Age rating mis-set (kids audience but 12+/17+ answers; or open web/UGC inside a kids app) | 2.3.6 |
| Chat/UGC inside a children's app without full moderation + child-safety posture | 1.2 escalation |
| Collecting child data "for analytics" beyond statutory needs | 5.1.1(iii), 5.1.4 |

## Audit specifics

- Dependency list × kids flag: flag **every** SDK that touches identifiers or ads. This includes SDKs pulled in transitively (ad mediation chains).
- `NSPrivacyTracking=false`, no `NSUserTrackingUsageDescription` in a kids app.
- Enumerate every external link (support, privacy policy, social, more-games) — each must sit behind the gate or be removed.
- Purchasing: IAP allowed but behind the parental gate area; subscription marketing must not target children visually.
- If the app is "for children" but not enrolled in Kids Category — Apple can still treat it as kids-primary from marketing; align metadata claims and data practices.

## Pre-submission checklist

- [ ] Zero third-party analytics/ads SDKs (or documented compliant ones only)
- [ ] All external links + purchases behind parental gate
- [ ] Privacy policy covers children's data; data collection minimized to statutory needs
- [ ] Age rating consistent with actual audience; Kids Category selection matches metadata claims
- [ ] Any UGC/chat has moderation + reporting appropriate for minors
