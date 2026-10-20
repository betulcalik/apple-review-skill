# Social Media & UGC Apps — Deep Dive

Social apps are the most-rejected app category relative to submissions, and the rejections cluster around a small set of rules that Apple enforces **literally**. This file audits everything Apple has historically rejected social apps for. If the inventory shows any UGC or social surface — posts, comments, profiles, follows, chat, DMs, communities, media uploads, reviews, AI-generated content shown to other users — this file applies in full, regardless of what the app calls itself.

**The five gates every social app passes through:** 1.2 (moderation features) · 2.3.6 (age rating) · 4.8 (login) · 5.1.1(v) (forced login, account deletion) · 3.1.x (monetization of digital interactions). Miss any one and the app bounces.

---

## 1. Guideline 1.2 — the four mandatory features

Apple checks these as a literal, binary checklist. Missing exactly one is still a rejection. From Apple's rejection template, the four required items are:

1. A method for **filtering objectionable material from being posted** to the app
2. A mechanism to **report offensive content and timely responses** to concerns
3. The ability to **block abusive users** from the service
4. **Published contact information** so users can easily reach you

### What counts as UGC (broader than developers think)

Comments · posts · photos/videos uploaded · profile bios and avatars · **usernames** · chat and DMs · community forums · reviews · audio rooms · shared AI generations · reactions visible to others · status messages · collaborative playlists · walls/guestbooks. If a second user can ever see something a first user wrote, it's UGC.

### Implementation bar per feature (what reviewers accept)

**Filter (post-side):**
- Text: keyword/profanity filter at input (client or server). Real, maintained wordlist; ideally also rate limiting and duplicate spam detection.
- Media: nudity/NSFW classification (on-device like Vision framework, or server-side API), with a moderation queue or auto-quarantine.
- Evidence to look for: filter service config, wordlists, moderation API keys, classifier calls in the posting pipeline.
- Common failure: "we moderate manually after the fact" with no posting-time mechanism — technically arguable, practically rejected.

**Report:**
- A visible report action attached to *every* unit of UGC (long-press menu, "…" button) and to user profiles.
- Reason codes (spam, harassment, sexual content, self-harm, other) — reviewers look for the picker.
- Confirmation to the reporter, and a real backend queue. "Timely responses" = you must be able to describe your moderation SLA; mention it in review notes ("reports triaged within 24h").
- Evidence: `report` endpoints, admin tooling references, UI strings.

**Block:**
- Block action from the profile and/or from content.
- After blocking: the blocked user's content disappears **for the blocker** in all surfaces (feed, comments, DMs), and the blocked user can't re-contact. If content still renders after block, Apple (and users) treat it as absent.
- Evidence: block API, feed query filters, DM sender filters.

**Contact info:**
- In-app: settings/about screen with support email or contact form — a real, monitored channel, not a bare website link.
- Also must satisfy 1.5 (support URL in ASC).

### The review-notes paragraph that prevents the round-trip

In App Review notes, state exactly where each feature lives, e.g.:

> - Report: "…" menu on every post/comment/profile, with reason codes; reports go to our moderation queue (responded within 24h).
> - Block: button on every profile; blocked users' content is hidden everywhere.
> - Filter: all text posts pass a profanity/keyword filter; images pass an NSFW classifier; flagged content is quarantined pending review.
> - Contact: Settings → Support (support@…), also in App Store metadata.

## 2. The worse 1.2 rejection — "used primarily for objectionable content"

Moderation features do **not** cure this one. If the app's actual usage becomes predominantly porn, anonymous harassment, or people-objectification, Apple removes it — possibly without notice. The named fatal patterns:

- **Chatroulette-style random video/audio chat**
- **Random or anonymous chat** (pairing strangers with no identity)
- **Objectification of real people** — "hot-or-not" voting, rating faces/bodies
- Apps **used primarily** for pornographic content, making physical threats, or bullying

**Structural risk factors to flag in an audit:** anonymous-by-default identity · stranger-matching as the core loop · photo-rating mechanics · no age verification while serving mature content · gender-selective matching in stranger chat (historically used for sexual pairing) · "disappearing" media + strangers · open live video with no entry checks.

**Mitigations that have demonstrably worked:** verified accounts/phone → identity + accountability · real-time nudity detection pausing streams · age gates · removing anonymity · proactive (not just reactive) moderation · keeping content persistence (accountability).

## 3. NSFW content rules

- In-app mature content requires an appropriate **age rating** (17+ typically) and it must still not be "overtly sexual/pornographic" (1.1.4 line).
- The **incidental NSFW carve-out**: a UGC app fed by a web service may show incidental mature content **only if hidden by default and enabled by the user on the website** — the toggle may not live in the app. This is the Reddit/Tumblr-style pattern. Verify: any in-app NSFW toggle violates the carve-out as written.
- Apps whose *primary* purpose is porn never qualify, toggle or not.

## 4. Minors & age

- **Age rating questionnaire (2.3.6):** social apps must answer honestly — UGC, unrestricted web content, mature themes, gambling simulation, alcohol references etc. A social app rated 4+ with open UGC is mis-rated and will bounce (either at review or later via user complaints). Real-world baseline: open UGC → 12+ minimum; DMs with strangers/mature content → 17+; often higher with frequent mature content.
- **1.2.1(a) creator apps:** must provide a way to identify content exceeding the app's age rating, and restrict access via verified/declared age.
- **Underage users:** if minors can be present, children's privacy statutes apply (5.1.4). Social features + minors + ads/analytics = COPPA exposure; consider age screening at signup and limiting data collection for detected minors.
- **1.3 Kids Category:** a social network for kids is near-impossible under 1.3 (no third-party analytics/ads, no links out without parental gate); treat "social app for children" as a P1 architecture risk, not a checklist item.

## 5. Login & account rules

- **4.8:** Facebook/Google/X/WeChat/etc. login for the primary account ⇒ must also offer Sign in with Apple (or an equivalent privacy-limited option) with **equivalent prominence**. Hidden "more options…" SIWA has been rejected repeatedly.
- **5.1.1(v):** if core functionality isn't a specific social network, users must be able to use the app without that network's login; sharing/invites/profile pulls are not "core". Mechanism to revoke social credentials in-app; social tokens stay on-device, used only for live connections to that network.
- **Account deletion (5.1.1(v)):** account creation ⇒ in-app initiated deletion. For social apps this typically means: Settings → Delete Account → confirm (re-auth) → server deletes profile + content per policy. A web-only form buried in a help center has been rejected. Deactivation-only flows have been rejected.
- Demo account (2.1): social apps always have login — provide a permanent, pre-populated demo account with visible content, excluded from OTP/rate limits.

## 6. Social-graph mechanics with their own rules

| Mechanic | Rule(s) | Failure mode |
|---|---|---|
| Find friends via Contacts | 5.1.2(iv)(v) | uploading the address book to your server = "building a contact database" violation; must use pickers or hashes matched server-side without storing raw contacts; no Select-All; no default-selected contacts; show the user what an invite message says and who sends it |
| Invites/referrals | 5.1.2(v), 3.2.2(x) | no auto-spam to contacts; no gating functionality on sends; incentive to invite is OK in-app, spamming is not |
| Push for messages/follows | 4.5.4 | transactional push fine; marketing push needs explicit opt-in + in-app opt-out; push never required for function; no sensitive/confidental content in payload |
| Blocking visibility | 1.2 | see above — must actually hide content |
| Muting/keyword blocking | 1.2 | good evidence of "filtering objectionable material" — surface in review notes |
| Live streaming | 1.2 + 2.5.14 | recording indicator (visual/audible) for broadcasts; comment moderation during stream; mature-content entry checks; high scrutiny — ship moderation tooling before features |
| Ephemeral content | 1.2 escalation | disappears ≠ unmoderable; still need report paths and retention for moderation |
| Community servers (Discord-like) | 1.2 | you own the moderation of communities hosted for you; report/block/contact must reach your service |

## 7. Monetization inside social apps

- **Coins/gems/tips/gifts** sent to creators or users = digital content = **IAP (3.1.1)**. External checkout for coins is the classic social-app 3.1.1 rejection.
- **Boosts/promoting posts** — explicitly named in 3.1.3(g): sales of boosts for posts in a social media app **must use IAP**.
- **Creator payouts** (real-money revenue share to creators): allowed as off-app economics, but the purchase of the digital units users give creators is IAP; "gifting" money person-to-person without digital-content tie-in and with 100% pass-through may avoid IAP (3.2.1(vii)) — the moment it's connected to receiving digital content/services, it's IAP.
- **Subscriptions** (premium social features, 3.1.2): ongoing value requirement — a one-time feature set behind a monthly sub gets "no ongoing value" rejections; paywall disclosure rules apply (title/length/price, auto-renewal statement, EULA + privacy links).
- **Ads in social feeds (2.5.18, 3.2.2(iii)):** labeled interstitials with visible close/skip, users can report ads, no targeting on sensitive data, no ads in extensions/widgets/App Clips.
- **Selling verification/badges** = digital feature = IAP.

## 8. Other frequent social-app rejections (quick table)

| Symptom | Guideline | Note |
|---|---|---|
| App is a thin client of one website / WebView wrapper of the social site | 4.2 | needs native navigation + device integration |
| "Another Instagram" with no differentiation | 4.3(b) | saturated category (incl. dating) — needs meaningfully different experience |
| Scraper of a social network (viewer for X/IG without API rights) | 5.2.2 | third-party service ToS; authorization on request |
| Downloading videos from social platforms | 5.2.3 | named fatal example |
| Sharing copyrighted music into posts/stories | 5.2.5 / 5.2.1 | needs rights-holder licenses (sync/adaptation) |
| Bot/follower/engagement manipulation | 3.2.2(vii), 5.6.3 | manipulating visibility/rank on other services |
| Fake follower/like apps | 5.6.3 discovery fraud | also 1.1.6 |
| Hookup apps | 1.1.4 | "may include pornography or facilitate prostitution" — high scrutiny of actual usage |
| Anonymous confession boards for schools | 1.2 escalation | bullying/anonymous abuse pattern |
| Political/interest communities without moderation capacity | 1.1.1 | groups targeting individuals; satirist exemption is narrow |
| Age questionnaire answered "no UGC" while app has comments | 2.3.6 | mis-rating — reviewers cross-check |

## 9. Social-app pre-submission checklist

- [ ] All four 1.2 features implemented, reachable, and listed in review notes with exact navigation paths
- [ ] Moderation queue staffed; report SLA documented
- [ ] Profanity/NSFW filtering active in posting pipeline (not just post-hoc)
- [ ] Block hides content everywhere (feed, comments, DMs, search)
- [ ] In-app contact info + working Support URL (1.5)
- [ ] Terms of Service / Community Guidelines published & linked at signup (moderation commitments, 1.2)
- [ ] Age rating questionnaire answered honestly (UGC questions); rating consistent with content policy
- [ ] NSFW (if any): hidden by default, toggle only on website
- [ ] SIWA (or equivalent) next to every social login, equal prominence (4.8)
- [ ] No forced social login; credential revocation in-app (5.1.1(v))
- [ ] Account deletion in-app, end-to-end (5.1.1(v))
- [ ] Demo account with content, excluded from OTP/rate limits (2.1)
- [ ] ATT flow before any ad SDK init; nutrition label matches actual SDKs (5.1.2, 5.1.1)
- [ ] All digital purchases (coins, boosts, subs, badges) via IAP; Restore Purchases present (3.1.1)
- [ ] Push: transactional vs marketing split; marketing opt-in + opt-out (4.5.4)
- [ ] Contacts usage: picker-based, no raw uploads, invite preview shown (5.1.2)
- [ ] No scraping/downloading third-party social content (5.2.2/5.2.3)
- [ ] Stranger-matching features reviewed against §2 fatal patterns; mitigations in place
- [ ] Privacy manifest complete incl. required-reason APIs; purpose strings specific
