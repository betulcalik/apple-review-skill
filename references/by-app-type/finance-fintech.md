# Finance & Fintech — App-Type Checklist

**Covers:** Finance, Banking, Investing, Budgeting, Lending, Insurance, Tax, Money Management, Trading tools.

## Top rejection reasons

| Risk | Guideline | Detail |
|---|---|---|
| Individual developer submitting a regulated finance app | 5.1.1(ix) | banking/financial services → submitted by the legal entity providing them |
| Missing licenses/permissions per region | 5.1.1(ix), 5.3-adjacent | document licenses in review notes per storefront availability |
| Binary options anywhere | 3.2.2(viii) | never permitted; CFDs/FOREX derivatives need licensing in every served jurisdiction |
| Personal loans without full disclosure | 3.2.2(ix) | all terms conspicuous; max APR ≤ 36% incl. costs/fees; no full repayment demanded ≤ 60 days |
| Financial trading app not from the institution | 3.2.1(viii) | must be submitted by the performing financial institution |
| Insurance app with IAP or a price tag | 3.2.1(v) | must be free, no IAP, legally compliant per region |
| Monetizing digital financial features with external checkout | 3.1.1 | premium budgeting tiers/subscriptions = IAP; only the regulated financial transaction itself may bypass |
| Account access without account features | 5.1.1(v) | read-only bank aggregators still need deletion flows for created accounts |
| Wallet passes misuse | 3.2.1(iv), 1.5 | only payments/offers/ID uses; issuer-signed with brand certificate + contact info |
| Apple Pay recurring charges without disclosures | 4.9 | term, what's provided, charges, cancel path |

## Audit specifics

- **Two money maps:** (1) the regulated money (accounts, transfers, trades) — institution eligibility + licenses; (2) the app's own monetization (premium tier) — IAP. Mixing them confuses reviewers; separate flows visibly.
- **Storefront × license matrix:** which regions is the app live in vs where licensed — geo-availability must match licensing.
- **Prominence rules:** loan APR/terms as conspicuous UI, not buried links.
- **Demo accounts:** sandbox bank logins or demo mode showing realistic accounts/transactions (2.1); reviewers can't open real accounts.
- **Data handling:** financial data is sensitive — ATS, encryption at rest (1.6), no PII in logs.

## Pre-submission checklist

- [ ] Organization/legal-entity account; licenses documented per storefront
- [ ] No binary options; derivatives licensed everywhere served
- [ ] Loan terms (if any) meet 3.2.2(ix) disclosures
- [ ] Premium features via IAP; regulated transactions via appropriate rails
- [ ] Insurance apps free without IAP
- [ ] Demo/bank-sandbox access for reviewers; Apple Pay recurring disclosures in place
