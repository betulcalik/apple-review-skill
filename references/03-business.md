# 3. Business — Guideline Section 3

The most expensive family to get wrong: fixing a 3.1 problem after review can mean rebuilding your paywall. The core principle: **if it unlocks features/functionality/content inside the app, it's in-app purchase.** Physical goods and a short list of exceptions escape.

## 3.1 Payments — IAP rules

### 3.1.1 In-App Purchase (the main rule)

You must use IAP to unlock: subscriptions, in-game currency, levels, premium content, full version. You may **not** unlock via license keys, AR markers, QR codes, crypto wallets, or your own mechanisms.

Also required by this rule:

- **Virtual currency must not expire**, and restorable purchases need a **restore mechanism** (a visible "Restore Purchases" button is the de-facto expectation).
- Gifting of IAP-eligible items is allowed; refunds only to the original purchaser; no exchange.
- **Loot boxes / randomized virtual items: disclose odds before purchase.**
- Digital gift cards/vouchers redeemable for digital goods → IAP only. Physical mailed gift cards → other payments OK.
- **Free trials before full unlock** (non-subscription): use a Non-Consumable IAP at Tier 0 named `"XX-day Trial"`; clearly state trial duration, what ends when the trial ends, and downstream charges, before the trial starts.
- NFTs: may sell NFT-related services via IAP; users may view their own NFTs; browsing others' collections allowed; **NFT ownership must not unlock app features**; outside the US storefront, no links/CTAs to non-IAP purchase mechanisms.
- Tips to developers/creators may use IAP currency.

Classic 3.1.1 rejections: Stripe/checkout web page for digital content · "coins" bought on a website spent in-app · license-key unlocks · expiring currency · missing Restore Purchases · loot boxes without odds · QR code that unlocks Pro.

### 3.1.1(a) External purchase links (post-DMA landscape, revised Oct 2026)

- **United States storefront:** buttons/external links/calls to action to non-IAP purchase mechanisms are permitted without an entitlement (per the 2025 injunction).
- **Specific other storefronts (EU etc.):** allowed only with the **StoreKit External Purchase Link Entitlement** (music streaming folded into the same mechanism in the Oct 2026 revision); destinations and disclosure modalities follow the entitlement agreement; the link may inform users where/how to buy and that prices may be lower.
- **Everywhere else:** no buttons/links/CTAs directing to non-IAP purchase mechanisms, in app or metadata.
- Misleading marketing/scams around the entitlement ⇒ removal + possible Program removal.

**Evidence:** entitlements file (`com.apple.developer.storekit.external-purchase` etc.), `openURL` targets to /pay|/checkout|/pricing|/subscribe, storefront availability lists in code, and whether the required entitlement-specific consent sheets are implemented (`ExternalPurchaseLinkAPI`).

### 3.1.2 Subscriptions

Any category may offer auto-renewable subscriptions, but:

| Rule | Requirement |
|---|---|
| 3.1.2(a) | **ongoing value**; period ≥ 7 days; works across all user's devices; users get what they paid for **without extra tasks** (no "post to social media", no contact upload, no check-ins to unlock what they bought); existing paid functionality must not be taken away when you switch business models; free trials configured in ASC with required disclosures; scam/bait-and-switch subscription flows = removal + possible Program removal |
| 3.1.2(b) | seamless upgrade/downgrade; users must not be able to **accidentally subscribe to multiple tiers** of the same thing (crossgrade/upgrade mechanics must downgrade, not stack) |
| 3.1.2(c) | **before** asking to subscribe: clearly describe what the price buys (how many issues, how much storage, what access); meet Schedule 2 of the ADPLA. In practice: title, length, price/period clearly and equally prominent; auto-renewal disclosure; links to Terms of Use (EULA) and Privacy Policy on the paywall |

Common 3.1.2 rejections: calculated pricing ("just $0.33/day!") more prominent than the billed amount · "Start Free Trial" button implying the app is free · missing auto-renew language · missing EULA/privacy links on the paywall · vague "Premium" without saying what's included · intro discount mislabeled as trial · paywall before any explanation of the subscription's value.

### 3.1.3 Other purchase methods (the exceptions — apply narrowly)

Apps using these exceptions still may not push users toward non-IAP purchase methods inside the app (US storefront and 3.1.1(a) excepted); out-of-app communications (email to your users) are fine.

| Rule | Exception | What it actually covers |
|---|---|---|
| 3.1.3(a) | "Reader" apps | previously-purchased content/subscriptions in magazines, newspapers, books, audio, music, video. Account creation/management for existing customers allowed. **External Link Account Entitlement** (apply in ASC) allows one informational link to account signup/management. Being a "reader" app is interpreted narrowly — a social or productivity app never qualifies |
| 3.1.3(b) | Multiplatform services | items bought on your website/other platforms must **also be available as IAP in the app** |
| 3.1.3(c) | Enterprise services | sold directly to orgs for employees/students (B2B/provisioning); consumer sales still need IAP |
| 3.1.3(d) | Person-to-person real-time services | **1:1 only** (tutoring, medical consult, fitness training). "One-to-few"/"one-to-many" (group classes, webinars) = IAP. The single most-abused exception; group class apps citing 3.1.3(d) get rejected |
| 3.1.3(e) | Physical goods/services consumed outside the app | use Apple Pay/card entry — **must** use non-IAP payments |
| 3.1.3(f) | Free stand-alone companions | to paid web tools for VoIP/storage/email/hosting; no purchasing or purchase CTAs in-app |
| 3.1.3(g) | Advertising management apps | advertisers buying/managing campaigns (TV, outdoor, web, apps) — management only, not displaying the ads. **Sales of "boosts" for posts in a social app = IAP, explicitly** |

### 3.1.4 Hardware-Specific Content

Features dependent on specific hardware may unlock without IAP (astronomy app + telescope). Optional companion of an approved physical product (toy) may unlock without IAP **provided an IAP option also exists**. You may not require purchases of unrelated products, advertising, or marketing activities to unlock.

### 3.1.5 Cryptocurrencies

| Rule | Constraint |
|---|---|
| (i) Wallets | organization accounts only |
| (ii) Mining | no on-device mining; cloud mining OK |
| (iii) Exchanges | only with proper licensing per country/region (license info in review notes) |
| (iv) ICOs/futures/quasi-securities | established banks/securities firms/FCMs/approved institutions only |
| (v) No crypto for tasks | no paying out crypto for downloads, referrals, social posting |

## 3.2 Other Business Models

**3.2.1 Acceptable (highlights):** cross-selling your own apps (not a mere catalog) · curated third-party app collections for a specific approved need with real editorial content · expiring approved rentals · permitted Wallet pass uses · insurance apps (free, no IAP) · approved-nonprofit fundraising with Apple Pay, fund-use disclosure, tax receipts, and per-nonprofit approval · **person-to-person gifts with no cut and no digital-content tie-in** · licensed financial institutions.

**3.2.2 Unacceptable:** App-Store-like storefronts for third-party apps · ad impression/click inflation; ad-saturated apps · charity fundraising without nonprofit approval (must collect outside the app) · arbitrary location/carrier restriction of who may use the app · manipulating a user's visibility/status/rank on other services against their ToS · binary options (never) · unlicensed CFDs/FOREX derivatives · personal loans: must disclose all terms incl. max APR ≤ 36%, no full repayment demanded in ≤ 60 days · **forcing ratings/reviews/downloads/other store actions to access functionality** (in-app incentives like completing a level or watching an ad are OK).

## Audit heuristics for this section

1. Find every money flow: what is bought, where, consumed where, by whom (digital vs physical, 1:1 vs group, consumer vs enterprise).
2. For each digital unlock → is there a StoreKit path? Any web checkout reachable in-app? Any entitlement needed and present?
3. Paywall audit against 3.1.2(c): disclosure completeness, prominence balance, EULA/privacy links, trial honesty.
4. Restore purchases present and visible? Loot boxes anywhere → odds displayed? Currency expiry anywhere?
5. "Tips"/"gifts"/"boosts"/"coins" vocabulary → check every one against the IAP-vs-gift rules (platform cut kills 3.2.1(vii); digital tie-in kills the gift exception).
6. Verify the exception being claimed is real: "reader" status, "p2p service" that is actually 1:many, "enterprise" that actually sells to consumers.
