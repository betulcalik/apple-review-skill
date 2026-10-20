# Lifestyle, Sports & Weather — App-Type Checklist

**Covers:** Lifestyle (habits, horoscopes, tattoo, wedding), Sports (scores, fandom, fantasy), Weather, Reference, Magazines-lite utilities, Fortune telling.

## Top rejection reasons

| Risk | Guideline | Detail |
|---|---|---|
| Fortune telling in the saturated list | 4.3(b) | horoscope/tarot apps need meaningful differentiation |
| Wallpaper/ringtones/simple timers spam | 4.3(b) | named saturated categories |
| Fart/burp/drinking-game/Kama Sutra apps | 4.3(b) | named low-quality genres; repeated submissions escalate |
| Sports betting paths without licensing | 5.3.4 | fantasy with cash prizes = real-money gaming regime |
| Live scores via unofficial feeds | 5.2.2 | league data (NFL/FIFA/etc.) licensing; authorization on request |
| Horoscope/prediction presented as real information | 1.1.6 | trick/false features — entertainment framing must be genuine |
| Weather data attribution missing | 5.2.5 | WeatherKit attribution requirements |
| Habit tracker requiring health data over-collection | 5.1.1(iii) | HealthKit for a habit streak app = over-asking unless core |
| Push marketing without opt-in | 4.5.4 | score alerts fine; promo pushes need consent + opt-out |
| Single-screen minimal apps | 4.2 | streak/counter apps need real functionality beyond one view |

## Audit specifics

- **Saturated-category defense:** first screen + release notes must show the differentiator (unique mechanics, depth, integration) — "another habit tracker" bounces under 4.3(b)/4.2.
- **Prediction claims:** entertainment framing must be structural (clearly playful UI), not a buried disclaimer (1.1.6 explicitly rejects "for entertainment purposes" labels as cure-alls).
- **Fantasy/prize structures:** entry-fee + cash-prize loops = gambling analysis (see `gambling-contests.md`); free-to-play with cosmetic rewards avoids it.
- **Sports data:** list every feed source × license; screenshots without league logos you can't use (5.2.1).
- **Widgets:** habit/score/weather widgets are natural — content must relate to the app (2.5.16) and carry no ads (2.5.18).
- **Subscription traps:** habit/routine apps commonly gate basic features behind trials — trial labeling and post-trial clarity (3.1.2) are the usual rejection.

## Pre-submission checklist

- [ ] Differentiation evident against the saturated-category bar
- [ ] Entertainment features framed as entertainment structurally
- [ ] Data feeds licensed; attribution where required
- [ ] No prize/cash mechanics unless licensed+geo+free per 5.3.4
- [ ] Widgets ad-free, on-topic; pushes opt-in for marketing
- [ ] Trial/subscription disclosures complete before subscribe
