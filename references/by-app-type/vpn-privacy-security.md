# VPN, Privacy & Security — App-Type Checklist

**Covers:** VPNs, ad blockers/DNS filters, parental controls, device security/antivirus-adjacent, MDM, device management, password managers, network tools.

## The regulated trio (VPN / MDM / parental control)

These are capability-gated, not just rule-governed:

| App | Rule | Bar |
|---|---|---|
| VPN services | 5.4 | `NEVPNManager` API only · **organization accounts only** · clear pre-use on-screen data declaration (before any purchase/use) · privacy policy commits to never selling/using/disclosing data to third parties · local-law compliance, license info in review notes where required |
| MDM | 5.5 | capability requested from Apple · commercial/education/government entities (limited parental-control/security companies) · pre-use data declaration · no data sale/disclosure · limited third-party analytics (app performance only, never user/device/app data) · **configuration-profile apps included** |
| Content blockers (Safari extensions) | 4.4.2 | current Safari · no System/Safari UI interference · website access no broader than necessary |
| Parental-control apps using MDM/VPN | 5.5/5.4 posture | Apple's limited-approved list; consumer parental apps avoiding MDM must not use sneaky alternatives (private API screen-time readers → 2.5.1 rejections) |

Violations = removal + alternative-distribution block + possible Program removal.

## Top rejection reasons

| Risk | Guideline |
|---|---|
| VPN from individual developer | 5.4 |
| VPN not using NEVPNManager | 5.4 |
| Missing pre-use data collection declaration screen | 5.4, 5.5 |
| Data-sharing language anywhere in VPN/MDM policy | 5.4, 5.5 |
| "Virus scanner" marketing that doesn't match function | 2.3.1(a) — iOS malware-scanner claims are a named example of misleading marketing |
| Fake device info / prank security alerts | 1.1.6 |
| Screen Time API monetization | 4.10 |
| Password manager uploading credentials | 5.1.1(v) posture — credentials stay on-device/user-controlled vaults |
| Network tools reading traffic without disclosure | 5.1.1, 5.1.2 |

## Audit specifics

- **Account type first:** VPN/MDM from an individual account is dead on arrival — check the submission account.
- **Declaration screens:** literally rendered before any purchase or connect action; reviewers screenshot them.
- **Policy text:** VPN/MDM policies must contain the no-third-party-data commitment — grep the privacy policy text for "sell", "share", "third part".
- **Security theater:** features like "jailbreak detection alerts", "privacy scores" must be backed by real measurements (1.1.6 false features).
- **Network extensions entitlements:** match declared extension types; unused entitlements trimmed (upload-time and 2.4.5(i)-style scrutiny).

## Pre-submission checklist

- [ ] Organization account; NEVPNManager for VPN; MDM capability granted by Apple
- [ ] Pre-use data declaration screen before purchase/connect
- [ ] Privacy policy with explicit no-sale/no-sharing commitment
- [ ] Marketing claims match measurable function (no scanner theater)
- [ ] Entitlement set minimal and justified
