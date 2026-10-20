# Media & Entertainment — App-Type Checklist

**Covers:** Entertainment, Video/Music streaming, News & Magazines, Books, Reader apps, Podcast/audio, Comic apps, Live-event apps.

## Top rejection reasons

| Risk | Guideline | Detail |
|---|---|---|
| Media downloading/recording from third-party sources | 5.2.3 | YouTube/Spotify/SoundCloud/Vimeo downloaders, converters — rejected even for streaming if it violates ToS |
| No rights for catalog content | 5.2.1, 5.2.2 | distribution licenses per content; authorization letter on request |
| Subscription paywall disclosure gaps | 3.1.2(c) | catalog size/cadence stated before subscribe (how many issues, hours of audio) |
| Claiming "Reader" status wrongly | 3.1.3(a) | reader = previously-purchased magazines/newspapers/books/audio/music/video only; social/utility apps never qualify |
| External account-signup link without entitlement | 3.1.3(a) | reader apps need the External Link Account Entitlement for that one link |
| Mature artwork in metadata | 2.3.8 | metadata 4+ regardless of 17+ catalog |
| Local content-rating compliance missing | 2.3.6 | film/music/game-style ratings display where territories require |
| Streaming quality/DRM stripping | 5.2 | no circumvention of protections |
| Live events metadata mismap | 2.3.13 | wrong dates across storefronts, broken deep links |
| Music playback misuse | 4.5.2 | MusicKit: user-initiated playback only, no monetization of Apple Music access, no file export/sharing; previews not used as soundtrack/BGM |

## Audit specifics

- **Rights ledger:** for every catalog source, note the license/permission basis — Apple asks on request (5.2.2); scraped/embedded feeds without documented permission = 5.2.3 risk.
- **Reader-app boundary:** if claiming reader status, the app may offer account creation for free tiers + management for existing customers only; one informational external link via entitlement; **no** purchase CTAs in-app (except US storefront rules).
- **Subscription honesty:** title/length/price disclosures, EULA + privacy links on the paywall; intro-price vs trial labeling (3.1.2).
- **Audio background modes** justified (2.5.4); offline downloads only for content you control or are licensed to cache.
- **AirPlay/Chromecast-style beaming** of third-party streams — licensing again.
- **Demo content:** reviewer-accessible sample catalog with playable media (2.1); region-lock the catalog honestly and tell reviewers which region has content.

## Pre-submission checklist

- [ ] Rights documented for every content source
- [ ] No download/convert functionality for third-party media
- [ ] Paywall disclosures complete; ToS + privacy links
- [ ] Reader status + entitlement claim verified (or not claimed)
- [ ] Content ratings displayed where required; metadata artwork 4+-safe
- [ ] Demo account with playable/streamable sample content per key region
