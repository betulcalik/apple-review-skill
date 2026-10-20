# All Apps — Universal Checklist

**Applies to:** every submission, any category. Run this file first, then the app-type file(s) that match.

## The universal rejection top-10 (by real-world frequency)

| # | Risk | Guideline | Check |
|---|---|---|---|
| 1 | Reviewer couldn't use the app: crash, empty screen, dead demo account, backend down | 2.1 | on-device test of the exact review path; demo credentials verified |
| 2 | Missing/vague privacy manifest or required-reason APIs undeclared | 5.1.1 / ITMS-91053 | `PrivacyInfo.xcprivacy` own + SDKs |
| 3 | Vague or missing permission purpose strings | 5.1.1(ii) / ITMS-90683 | every `NS*UsageDescription` specific and app-named |
| 4 | Privacy policy missing in ASC metadata or not easily reachable in-app | 5.1.1(i) | both locations |
| 5 | Account creation without in-app account deletion | 5.1.1(v) | delete flow initiated inside the app |
| 6 | Paywall/subscription disclosure gaps | 3.1.2(c) | title, length, price, auto-renewal language, ToS + privacy links |
| 7 | Metadata inaccuracy: screenshots not app-in-use, keyword stuffing, name issues | 2.3.3/2.3.7 | per-device screenshots; clean keywords |
| 8 | IAP broken/incomplete during review (products "Missing Metadata", not submitted with binary) | 2.1(b), 3.1.1 | sandbox purchase pass; Restore Purchases present |
| 9 | Private/deprecated APIs | 2.5.1 / ITMS-90809 | upload gate sweep |
| 10 | 4.2 minimum functionality (WebView wrapper, marketing app) | 4.2 | native functionality beyond the site |

## Universal checklist

- [ ] Final build — no placeholders, test fixtures, "coming soon", Lorem ipsum
- [ ] Demo account (if login): permanent, pre-verified, no OTP, excluded from rate limits; or approved demo mode
- [ ] Backends/staging up during the review window
- [ ] App icon complete (all sizes + 1024 no-alpha), launch screen present
- [ ] `What's New` specifically describes significant changes; review notes describe new features with locations
- [ ] Age rating questionnaire answered honestly; metadata 4+-appropriate regardless of app rating
- [ ] No other-platform references (Android, alt marketplaces) in app or metadata
- [ ] Rights for all assets; fictional account data in screenshots (never real users)
- [ ] IPv6-only network works (2.5.5); no unused background modes (2.5.4)
- [ ] Export compliance answered / `ITSAppUsesNonExemptEncryption`
- [ ] Support URL live; contact path inside the app (1.5)
- [ ] No hidden/dormant features or review-environment detection (2.5.19)
- [ ] No forced ratings/reviews to unlock functionality (3.2.2(x)); use `SKStoreReviewController`
- [ ] Push never required; marketing push opt-in + opt-out (4.5.4)
