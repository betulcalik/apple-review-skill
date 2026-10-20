# Crypto & Web3 — App-Type Checklist

**Covers:** Cryptocurrency wallets, exchanges, ICO platforms, NFT apps, DeFi dashboards, mining-adjacent tools, blockchain explorers, token-gated communities.

## Top rejection reasons

| Risk | Guideline | Detail |
|---|---|---|
| Wallet app from an individual account | 3.1.5(i) | wallets: organization enrollment only |
| On-device mining (incl. via embedded ads) | 3.1.5(ii), 2.4.2 | never; cloud mining OK |
| Exchange without regional licensing | 3.1.5(iii) | license info in review notes; availability geo-matched to licenses |
| ICO/futures/quasi-securities from non-institutions | 3.1.5(iv) | established banks/securities firms/FCMs/approved institutions only |
| Crypto payout for tasks | 3.1.5(v) | no rewards for downloads/referrals/social posts |
| NFT ownership gating app features | 3.1.1 | external NFT state must not unlock in-app functionality |
| NFT purchase CTAs to external checkout (non-US storefronts) | 3.1.1 | outside the US, selling NFTs/minting as a service → IAP only, no external purchase links |
| Crypto trading submitted by individual | 5.1.1(ix) | regulated field → legal entity |
| Token-gating hidden features | 2.5.19 | unlockable "later" features = hidden functionality |

## Audit specifics

- **Feature split:** custody/wallet vs exchange vs content (NFT viewing). Wallets = org account; exchange services = licensing story; viewing others' collections = allowed with limits (no purchase CTAs outside US storefront).
- **Monetization:** minting/listing/transferring **as a paid service** = IAP (3.1.1 NFT clause). Gas fees paid on-chain by users acting on their own assets are part of the transaction, not an unlock fee — keep the UI distinction clear.
- **Storefront matrix:** external-purchase-link entitlements and US rules (3.1.1(a)) apply per storefront; default deny external links everywhere else.
- **Seed phrase / key handling:** never uploaded, never logged, encrypted at rest (1.6); screenshots/preview metadata must not show real wallets (2.3.9).
- **Community/DAO features:** token-gated chat/feeds = UGC (1.2 four features) + no anonymous abuse patterns — see `social-ugc.md`.
- **Airdrop/reward mechanics:** "do X, earn token" = 3.1.5(v) violation.

## Pre-submission checklist

- [ ] Organization account; wallet/exchange eligibility documented
- [ ] No on-device mining anywhere (audit embedded ad SDKs too)
- [ ] Licensing per storefront, license info in review notes
- [ ] NFT: no feature-unlocks from ownership; purchase services via IAP outside US; no external CTAs in other storefronts
- [ ] No task-based crypto rewards
- [ ] Seed-phrase handling secure; demo wallet view for reviewers (read-only, seeded)
