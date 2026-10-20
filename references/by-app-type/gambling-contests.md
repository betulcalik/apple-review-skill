# Gambling, Betting & Contests — App-Type Checklist

**Covers:** real-money gaming (casino, sports betting, poker, horse racing), lotteries, sweepstakes, contests, raffles, social casino, prize competitions.

## Top rejection reasons

| Risk | Guideline | Detail |
|---|---|---|
| Real-money gaming without licensing + geo-restriction + free app | 5.3.4 | all three required together, in every served location |
| IAP buying credit for real-money play | 5.3.3 | never — deposits go through non-IAP rails for licensed apps |
| Sweepstakes not sponsored by the developer | 5.3.1 | official rules name the app's developer as sponsor |
| Rules missing "Apple is not a sponsor" | 5.3.2 | stated in-app, in the official rules |
| Lottery missing consideration/chance/prize structure | 5.3.4 | structure + local law |
| Card counters / gambling aids | 5.3.4 | "illegal gambling aids" banned |
| Social casino with cash-out paths | 5.3.4 posture | any real-money loop converts it to real-money gaming |
| Simulated gambling undeclared in age rating | 2.3.6 | questionnaire answer bumps rating (17+ typically) |
| Paid-entry contests with prizes as digital goods | 3.1.1/5.3 mix | entries/prizes structured carefully; skill-vs-chance analysis |

## Audit specifics

- **Money-flow classification:** where funds enter (IAP vs external), what they buy (chips for real-money tables vs entertainment currency), and how (if ever) funds exit. Any exit = real-money gaming regime: license, geo-availability matrix, free app, non-IAP deposits.
- **Sweepstakes/contest mechanics in non-gambling apps** (marketing campaigns, giveaways): sponsor = developer (5.3.1), rules in-app with the Apple-not-a-sponsor line (5.3.2), no purchase-as-consideration structures where jurisdictions forbid, age/geo eligibility stated.
- **Loot-box-adjacent prize wheels** in any app: if purchasable, odds disclosed pre-purchase (3.1.1).
- **Skill competitions with entry fees:** 1:1 or tournament formats still need local-law diligence; Apple defers to licensing — document in review notes.
- **Age rating + metadata:** simulated gambling questionnaire answers; no casino imagery in 4+ metadata contexts.
- **Demo mode:** reviewer access to table/gameplay without depositing; geofence explained in review notes with the license list.

## Pre-submission checklist

- [ ] Real-money flows: license × geo-availability × free app × non-IAP deposits — all consistent
- [ ] No IAP-to-real-money-credit path anywhere
- [ ] Sweepstakes: developer-sponsored, in-app rules, Apple-not-sponsor clause
- [ ] Gambling aids removed; simulated gambling declared in rating
- [ ] Review notes carry licenses + demo access + geo rationale
