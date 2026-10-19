# 1. Safety — Guideline Section 1

Apple's framing: apps must not contain upsetting/offensive content, damage devices, or risk physical harm. Safety violations are also the ones that can end with **removal without notice and Developer Program termination** — treat them as more dangerous than they look.

Severity defaults in this file are floors, not ceilings — raise them when the pattern is egregious.

---

## 1.1 Objectionable Content

What it covers: offensive, insensitive, upsetting, disgusting, poor-taste, or "creepy" content — Apple explicitly reserves judgment ("we'll know it when we see it").

| Rule | Topic | What gets rejected in practice |
|---|---|---|
| 1.1.1 | Defamatory/discriminatory/mean-spirited content | content targeting religion, race, sexual orientation, gender, national/ethnic origin or other groups; harassment of targeted individuals. Political satirists/humorists generally exempt |
| 1.1.2 | Realistic violence | realistic killing/maiming/torture of people or animals; content encouraging violence; game "enemies" that map to a real race/culture/government/corporation |
| 1.1.3 | Weapons | encouraging illegal/reckless weapon use; facilitating firearm/ammunition purchase |
| 1.1.4 | Sexual/pornographic material | explicit sexual content; "hookup" apps; apps facilitating prostitution, human trafficking, exploitation. The classic killer of random video-chat and AI-companion apps |
| 1.1.5 | Religious commentary | inflammatory religious content; inaccurate/misleading scripture quotations |
| 1.1.6 | False information/features | fake device data, trick/joke functionality (fake location trackers, fake police sirens), anonymous/prank calls or SMS. "For entertainment purposes" does **not** save it |
| 1.1.7 | Profiting from tragedies | capitalizing on violent conflicts, terrorist attacks, epidemics |

**Evidence approach:** most 1.1 findings come from *content*, not code. Check: bundled assets (`Assets.xcassets`, media files), sample/seed content shipped in the binary, default usernames, test fixtures with real-looking harassment, easter eggs, loading-screen copy, and the app's own marketing site if listed. For apps fetching remote content, the story changes to 1.2 (UGC) — moderated, filtered, age-gated.

**Special case — AI-generated content:** apps that generate images/text/chat must add their own safety filters (output moderation, blocked prompt categories) or expect 1.1.4/1.2 scrutiny. Apple treats AI outputs as content the app publishes.

---

## 1.2 User-Generated Content (UGC) — the social-app gate

> Any app where users can create, generate, upload, share, or otherwise contribute content: social networks, UGC platforms, AI content creation apps, comments, reviews, profiles, chat, DMs, communities, even custom usernames.

### The four mandatory features — Apple checks these as a literal checklist

1. **Filter objectionable material from being posted** — profanity/keyword filters on text, NSFW/media classification on images, server- or client-side. It does not need to be perfect; it needs to exist and be real.
2. **Mechanism to report offensive content + timely responses** — a visible report action on every unit of UGC (post, comment, profile, message), with reason codes, and an actual triage process behind it.
3. **Ability to block abusive users** — block from profile or from any piece of content; blocked users' content disappears for the blocker.
4. **Published contact information** — reachable contact (email/support page) inside the app, not only on a website.

Missing any one of the four = rejection, essentially automatically. See `references/social-media.md` for the full deep-dive: implementation guidance, code-evidence patterns, review-note phrasing, and the worse 1.2 rejection ("app used primarily for objectionable content") that moderation features do not cure.

### Removal-without-notice patterns (moderation does not save these)

Apps **used primarily for**: pornographic content · Chatroulette-style random chat · random or anonymous chat · objectification of real people ("hot-or-not" voting) · physical threats · bullying.

### Incidental mature "NSFW" content

A UGC app pulling from a web-based service may display incidental mature content **only if**: hidden by default, and enabled by the user on the website (not in-app), with the app otherwise compliant.

### 1.2.1 Creator Content & 1.2.1(a) age gating (new in the Nov 2025 / Oct 2026 revisions)

Apps that aggregate content from a "creator" community (video, articles, audio, casual games) are treated as UGC apps: full 1.2 moderation required, plus 3.1.1 for any creator monetization. **1.2.1(a):** creator apps must let users identify content exceeding the app's age rating and must restrict underage access via verified or declared age.

### Enforcement escalation

Fail to safeguard, or fail to remediate after Apple's notice ⇒ treated as a Developer Code of Conduct violation: app pulled until you demonstrate improvements; egregious/repeat behavior ⇒ app removal + Developer Program removal.

---

## 1.3 Kids Category

Opt-in category with stiff requirements — and they stick: once users expect Kids compliance, deselecting the category later doesn't waive it.

- No links out of the app, purchasing opportunities, or other distractions **unless** behind a designated parental-gate area.
- No third-party analytics or third-party ads. Limited exceptions: analytics that don't collect/transmit IDFA or anything identifying children (name, DOB, email, location, device/network info); contextual ads only from providers with documented Kids practices including human review of creatives.
- Kids apps may not send PII or device information to third parties.
- Must comply with children's privacy law (COPPA, GDPR-K, etc. — see 5.1.4).

**Evidence:** dependency list × kids flag. Any Firebase/Facebook/Adjust/AdMob/etc. SDK in a Kids-targeted app is a P0/P1. Check `NSPrivacyTracking`, IDFA usage, and whether a parental gate actually exists (UI math/hold-to-continue patterns are the accepted implementations).

## 1.4 Physical Harm

| Rule | Trigger | Practical bar |
|---|---|---|
| 1.4.1 | Medical apps with inaccurate data/diagnosis/treatment | claims like blood pressure/BP/temperature/glucose/SpO2 from device sensors alone = rejected. Disclose data + methodology for health-measurement claims; remind users to consult a doctor; attach regulatory clearance docs |
| 1.4.2 | Drug dosage calculators | must come from manufacturer/hospital/university/insurer/pharmacy/approved entity, or have FDA-equivalent approval |
| 1.4.3 | Tobacco/vape/illegal drugs/excessive alcohol | encouraging consumption = rejected; encouraging minors = fast rejected; facilitating sale of controlled substances/tobacco banned (licensed cannabis dispensaries geo-restricted excepted) |
| 1.4.4 | DUI checkpoints | only law-enforcement-published data; never encourage drunk driving/reckless speed |
| 1.4.5 | Risky challenges/bets | urging users toward physically risky activities or device misuse |

## 1.5 Developer Information

App **and** its Support URL must include an easy, working way to contact you — especially classroom-relevant apps. Broken/dead support URL is a common, cheap rejection. Wallet passes must carry valid issuer contact info and be signed with the brand/trademark owner's dedicated certificate.

**Evidence:** support URL from ASC metadata (NEEDS-VERIFY) + an in-app settings/about screen with contact path. Check for mailto:, contact forms, support links in string catalogs.

## 1.6 Data Security

"Appropriate security measures" for handling user data — feeds 5.1. In practice App Review + automated scans look for: plaintext secrets in the binary (API keys, tokens — see preflight grep), unencrypted transmission (ATS exceptions, `http://` endpoints), credentials stored unencrypted in UserDefaults/plist, private data written to logs.

**Evidence:** `NSAppTransportSecurity` exceptions, hardcoded keys, `print`/`os_log` of PII, receipt-validation strategy (server-side vs none).

## 1.7 Reporting Criminal Activity

Apps for reporting alleged crime must involve local law enforcement, only in regions where that involvement is active.

---

## Cross-cutting Safety notes

- Safety is where Apple's automated systems + web crawlers also look at your **marketing site and support pages** — objectionable content there counts (real rejections cite "app marketing").
- 1.1.x violations inside **metadata** (screenshots, description) get processed as metadata rejections (2.3.x family) — same content, different fix path (no binary rebuild).
- Games: check the actual enemy/target representations (1.1.2) and any user-facing "kill X" copy.
- Age rating interplay: content that's fine at 17+ (mature themes) still must not be in metadata (2.3.8 metadata is always 4+).
