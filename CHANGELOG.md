# Changelog

## 1.0.0 — 2026-10-20

Initial release.

- Full coverage of App Store Review Guidelines sections 1–5, current through the October 16, 2026 revision (creator-content age gating 1.2.1(a), creative assets 2.3.14, hidden features 2.5.19, revised external-purchase-link entitlement rules 3.1.1(a), human-trafficking/exploitation escalation in section 5).
- Evidence-based audit workflow: stack detection, risk-surface inventory (Info.plist, entitlements, privacy manifests, SDKs), grep battery, P0–P3 severity, NEEDS-VERIFY for non-verifiable items.
- Social media / UGC deep dive: the 1.2 four-feature checklist, objectionable-use escalation patterns, NSFW web-toggle carve-out, age ratings, Sign in with Apple, forced-login and account-deletion rules, contact-upload rules, social monetization (coins/boosts/tips/gifts), push rules, stranger-matching risk factors, pre-submission checklist.
- Rejection case database keyed by guideline with response-type classification (binary / metadata / information-needed / removal).
- Upload validation (ITMS-*) reference and pre-upload sweep.
- Rejection-response playbook: Resolution Center reply structure and templates, appeals, expedited review, bug-fix submissions.

## 1.1.0 — 2026-10-20

Category checklists: full by-app-type coverage.

- New `references/by-app-type/` with 19 category deep-dives + index:
  all-apps (universal), social-ugc (moved from references/social-media.md),
  games, kids-family, education, health-fitness, finance-fintech,
  crypto-web3, ecommerce-marketplaces, media-entertainment,
  photo-video-creative, dating, ai-apps, vpn-privacy-security,
  gambling-contests, business-productivity, macos-platforms, travel-food,
  lifestyle-sports-weather.
- SKILL.md Step 0 now routes detected app categories to their checklists
  (categories stack); deep-dive triggers rewritten accordingly.
- README: "By app category" index table added.
