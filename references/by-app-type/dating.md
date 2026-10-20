# Dating — App-Type Checklist

**Covers:** Dating, matchmaking, speed-dating, "meet new people" apps. Heavily overlaps `social-ugc.md` — read both.

## Why dating is special

Apple explicitly names dating among **saturated categories** (4.3(b)) — new submissions must be meaningfully different — and dating's core mechanics (stranger matching, photos, chat, location) trip 1.2's fatal patterns and privacy rules simultaneously.

## Top rejection reasons

| Risk | Guideline | Detail |
|---|---|---|
| Missing any of the 1.2 four (filter/report/block/contact) | 1.2 | table-stakes for chat/profiles |
| Anonymous/random matching as the core loop | 1.2 escalation | Chatroulette-style pairing, hot-or-not rating |
| Age rating below reality | 2.3.6 | dating with open chat/photos = 17+ typically; questionnaire must match |
| Underage access without age gate | 1.2.1(a)-adjacent, 5.1.4 | verified/declared age gating; minors present = children's privacy statutes |
| Location used for matching without clear consent/purpose | 5.1.5 | consent + in-app purpose explanation; no always-on tracking |
| Contacts-based friend finding | 5.1.2(iv)(v) | no raw uploads; picker-based; no select-all; invite preview |
| Premium messaging/boosts sold outside IAP | 3.1.1 | coins, super-likes, boosts, visibility tiers = IAP |
| No account deletion | 5.1.1(v) | dating users demand it; in-app initiated, end-to-end |
| Social login without SIWA equivalent | 4.8 | phone-number-only login is fine (own system); FB/Google login needs SIWA |
| Screenshot marketing with real users | 2.3.9 | fictional profiles only |
| "Just another Tinder" | 4.3(b) | differentiation must be demonstrable |

## Audit specifics

- **Moderation depth:** dating needs the full 1.2 stack **plus** demonstrable anti-harassment posture: photo verification flows, report SLAs, banned-user handling. Escalation risk is real — apps "used primarily" for harassment get pulled without notice.
- **Safety features reviewers like:** video-verified profiles, in-app safety tips, blocking that hides everywhere (feed, chat, search), panic/emergency guidance for meetups.
- **Monetization map:** swipes/boosts/coins/visibility = IAP; subscription paywall disclosures (3.1.2(c)) with what's included (how many boosts, message limits).
- **Age & rating story:** signup age gate (declared or verified), rating questionnaire answers consistent (unrestricted chat + mature themes → 17+), no "for teens" marketing ever.
- **Demo environment:** populated candidate feed + working chat for the reviewer account — empty feeds read as broken (2.1).

## Pre-submission checklist

- [ ] 1.2 four features + verification/safety flows
- [ ] No anonymous-random core mechanics; identity/accountability present
- [ ] Age gate at signup; rating questionnaire honest (typically 17+)
- [ ] Location consent + purpose explained in-app
- [ ] All premium mechanics via IAP; disclosures complete
- [ ] In-app account deletion; SIWA next to social logins
- [ ] Differentiation evident vs category leaders (for 4.3(b))
- [ ] Demo feed/chat populated for review
