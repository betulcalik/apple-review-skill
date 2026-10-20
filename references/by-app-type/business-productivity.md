# Business & Productivity — App-Type Checklist

**Covers:** Business, Productivity, Utilities, Developer tools, SaaS clients, CRM/project management, remote desktop, team communication.

## Top rejection reasons

| Risk | Guideline | Detail |
|---|---|---|
| Consumer features priced via external checkout | 3.1.1 | premium digital tiers (more projects, exports, AI features) = IAP |
| Enterprise exception claimed for consumer sales | 3.1.3(c) | only direct-to-org sales (employees/students) bypass IAP; single-user consumer sales may not |
| Remote desktop not following 4.2.7 | 4.2.7 | user-owned host only, LAN-based, execution on host, host-side account creation, no store-like UI, no thin clients for cloud apps |
| Team accounts the reviewer can't exercise | 2.1 | demo workspace with members/tasks/docs populated |
| "Reader"-style excuses for SaaS | 3.1.3(a) | SaaS ≠ reader category |
| Data collection beyond utility need | 5.1.1(iii) | scanners/importers pulling more than the feature requires |
| Browser/email-client login rules | 4.8 exemptions | client-of-a-specific-service exempt from SIWA; generic tools with social login are not |
| Forced review for team features | 3.2.2(x) | no gating on ratings |
| Push required for operation | 4.5.4 | degrade gracefully without push |
| Own-account-system + email harvesting | 5.1.1(x) | optional name/email only; features not conditional |

## Audit specifics

- **Seat-based SaaS:** web-purchased seats usable in-app (multiplatform/enterprise posture) is common and accepted **if** the app is genuinely a service companion (3.1.3(b)/(c) logic) — but in-app purchase CTAs for those digital seats follow 3.1.1 unless the enterprise exception genuinely applies. Document the model in review notes; unexplained models delay review (Section 3 preamble).
- **Utility honesty:** file cleaners, "boosters", battery improvers must do measurable things (1.1.6); fake scans are named deceptive examples.
- **Team communication features:** channels/comments = UGC (1.2 stack) even in enterprise apps — Apple applies 1.2 to team chat too when accounts are free-form.
- **Exports/integrations:** share sheet usage; Contacts/Calendar access via out-of-process pickers where possible (5.1.1(iii)).
- **Demo workspace:** seeded org with realistic content + a documented test login (2.1) — biggest practical rejection for B2B apps is the empty demo.

## Pre-submission checklist

- [ ] Monetization model explained in review notes (who buys, where, what's digital)
- [ ] Consumer digital tiers via IAP; enterprise exception documented if claimed
- [ ] Remote desktop (if any) meets 4.2.7 a–e
- [ ] Demo workspace populated; test credentials documented
- [ ] Team chat/comments carry 1.2 features
- [ ] No rating-gated features; push optional
