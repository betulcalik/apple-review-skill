# Games — App-Type Checklist

**Covers:** all Games subcategories (incl. casino/simulation, hyper-casual, streaming game services, retro emulators).

## Top rejection reasons for games

| Risk | Guideline | Detail |
|---|---|---|
| Loot boxes / randomized purchases without disclosed odds | 3.1.1 | odds shown **before** purchase |
| Expiring purchased currency | 3.1.1 | purchased in-game currency may not expire |
| No Restore Purchases | 3.1.1 | non-consumables + subscriptions must be restorable |
| Real-money gambling without licensing/geo-restriction/free app | 5.3.4 | casino games with cash out = real-money gaming; social casino OK but no cash paths |
| "Enemies" that map to real groups/corporations/governments | 1.1.2 | also violence realism vs age rating (2.3.6) |
| Streaming game catalogs / emulators not following 4.7 rules | 4.7 | hosted games must follow IAP + privacy rules, be downloadable from App Store, age-restricted if mature |
| Beta/broken levels, placeholder art mid-game | 2.1 | reviewers play past the tutorial — every core loop must work |
| Game controller required but not disclosed | 2.4.3 | Apple TV especially; Siri Remote must suffice unless metadata says otherwise |
| Multiple near-identical games from one account | 4.3(a) | reskins under different Bundle IDs = spam |
| Fake "multiplayer"/bots presented as players | 1.1.6 | deceptive features |
| Kids game with third-party analytics/ads | 1.3 / 5.1.4 | no IDFA, no child identifiers, or remove |
| Leaderboards/cheat manipulation sold | 3.2.2(vii) | manipulating rank on services |
| Violence/gore in screenshots | 2.3.8 | metadata always 4+, pick neutral frames |

## Game-specific checks

- **Monetization map:** every purchasable → consumable/non-consumable/sub; currency ledger (no expiry); odds screens before loot boxes; gift mechanics (3.1.1: refundable only to purchaser, non-exchangeable).
- **Age rating questionnaire:** violence frequency/realism, gambling simulation ("simulated gambling" bumps rating), mature themes — mismatch between gameplay and questionnaire is a common metadata rejection.
- **Game Center:** Player IDs never displayed/shared (4.5.5); no spam via Game Center (4.5.3).
- **Cross-platform play:** items bought elsewhere must also exist as IAP in-app (3.1.3(b)).
- **External accounts:** game login via third-party (e.g. platform accounts) → 4.8 SIWA equivalent unless client-of-service exemption applies.
- **Streaming/emulator offerings:** you own compliance for every hosted title (4.7.1–4.7.5): no native API exposure to hosted content, universal-link index, age gates for mature mini games.

## Pre-submission checklist (games)

- [ ] Odds disclosed before every randomized purchase
- [ ] Currency never expires; Restore Purchases visible in settings
- [ ] All core loops playable with demo account; no dead levels
- [ ] Violence level consistent with age rating + screenshots 4+-safe
- [ ] No real-money gaming unless licensed, geo-restricted, free app (else remove cash-out)
- [ ] Emulator/streaming catalog compliant with 4.7 sub-rules
- [ ] No reskin siblings under other Bundle IDs
- [ ] Controller requirements disclosed (TV) or Siri Remote suffices
