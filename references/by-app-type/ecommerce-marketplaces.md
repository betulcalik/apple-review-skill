# E-commerce & Marketplaces — App-Type Checklist

**Covers:** Shopping, Food & Drink delivery, Marketplace/Classifieds, Local services, Booking, Loyalty/wallet apps, dropshipping storefronts.

## Top rejection reasons

| Risk | Guideline | Detail |
|---|---|---|
| Using IAP for physical goods | 3.1.3(e) | physical goods/services consumed outside the app **must** use non-IAP payments (Apple Pay / cards) — the inverse mistake |
| Using external checkout for digital goods | 3.1.1 | premium membership tiers, digital gift cards, "pro features" = IAP |
| Digital gift cards sold via card checkout | 3.1.1 | redeemable-for-digital vouchers = IAP only; mailed physical cards exempt |
| Marketplace with no functioning demo catalog | 2.1 | reviewer must browse real listings with prices; empty stores bounce |
| Order/account flows the reviewer can't traverse | 2.1 | provide demo mode or test environment (test payment sandbox visible without real charge) |
| Vendors selling prohibited items | 1.1.3, 1.4.3 | weapons, tobacco, controlled substances across the catalog — you own the listings |
| Counterfeit/IP-infringing goods | 5.2.1 | marketplace liability for listings (rights complaints) |
| Listings with misleading prices | 3.0, 5.6 | bait pricing, rip-off shipping tricks |
| Storefront for installing third-party apps | 3.2.2(i) | app-catalog-like interfaces banned |
| Person-to-person "gifts" where platform takes a cut | 3.2.1(vii) | tips/gifts tied to digital content = IAP |

## Audit specifics

- **Two-bucket money map:** every purchasable classified physical (external checkout OK/required) vs digital (IAP required). Mixed carts need split checkout — common architecture gap discovered late.
- **Membership/subscription tiers** (free shipping clubs, premium listings) = digital = IAP (3.1.1/3.1.2), paywall disclosures included.
- **Booking commissions** for real-world services consumed outside the app (restaurants, salons, tours): person-to-person services are 1:1-only (3.1.3(d)); marketplace-style booking fees generally fall under goods/services outside the app (3.1.3(e)) — document the rationale in review notes.
- **Moderation obligations:** classifieds/UGC listings = full 1.2 stack (report/block/filter/contact) — see `social-ugc.md`.
- **Loyalty/wallet passes:** permitted uses only (payments, offers, ID); issuer-signed certificates (3.2.1(iv), 1.5).
- **Demo environment:** seeded catalog, dummy payment step the reviewer can complete, order history populated.

## Pre-submission checklist

- [ ] Every SKU classified physical vs digital; checkout split accordingly
- [ ] Digital tiers via IAP with full subscription disclosures
- [ ] Catalog moderated (prohibited items, counterfeits); UGC features = 1.2 stack
- [ ] Demo store with browsable inventory + completable dummy checkout
- [ ] No app-store-like surfaces for third-party apps
- [ ] Wallet passes issuer-signed with valid contact info
