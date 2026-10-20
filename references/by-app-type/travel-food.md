# Travel, Navigation & Food — App-Type Checklist

**Covers:** Travel, Navigation, Food & Drink (delivery/reservation), Transportation/rideshare, Hotels, Airlines, Transit, Maps tools, event/local discovery.

## Top rejection reasons

| Risk | Guideline | Detail |
|---|---|---|
| Booking fees handled with the wrong payment rail | 3.1.3(d)/(e) | real-world services consumed outside the app → external payments; 1:1 services exempt from IAP, one-to-many (group tours) not |
| Location requested beyond feature need | 5.1.5, 5.1.1(iii) | always-on location for a guide app = rejection; when-in-use for discovery |
| Location gating functionality without consent clarity | 5.1.2(i) | no denying features because user declined tracking/location; offer manual entry |
| Content empty in reviewer's region | 2.1 | US-based reviewer + city-locked inventory + no demo = "app doesn't work"; document region + demo data |
| Transit/navigation encouraging misuse | 1.4.4-adjacent | DUI checkpoint rules (law-enforcement data only), speeding encouragement |
| Scraped POI/booking data | 5.2.2 | third-party service content needs ToS permission (Google Maps/Yelp/TripAdvisor data) |
| Ride/fare estimates misleading | 2.3.1 | price claims must match reality |
| Boarding passes / Wallet misuse | 3.2.1(iv) | passes for payments/offers/ID only; issuer-signed |
| Background location without justification | 2.5.4 | navigation apps only, declared mode |
| No offline degradation | 4.0/2.1 | dead spinner with no connectivity reads as broken |

## Audit specifics

- **Region strategy:** enumerate storefronts vs inventory coverage; either geo-limit availability honestly or ship universal content + demo region; review notes state where content lives.
- **Location economy:** when-in-use default; background only for active navigation with declared `UIBackgroundModes` + visible indication; consent copy explains matching/discovery purpose in-app (5.1.5).
- **Booking money map:** commissions/fees for real-world services (rides, tables, rooms) → external rails are correct (3.1.3(e)); premium digital tiers (ad-free guide, saved trips sync) → IAP.
- **Deep data sources:** licenses for POI/review content; attribution where required (WeatherKit-style).
- **Demo:** seeded city with bookable inventory; test payment sandbox; sample QR/code flows (2.1 demo resources).

## Pre-submission checklist

- [ ] Region × content matrix documented; demo data for reviewer's region
- [ ] Location: when-in-use default, purpose explained, graceful denial path
- [ ] Booking fees on correct rails; digital tiers via IAP
- [ ] POI/review data licensed; no scraping
- [ ] Background modes justified; offline states designed
- [ ] Wallet passes correctly scoped and signed
