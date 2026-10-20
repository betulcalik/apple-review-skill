# Health, Fitness & Medical — App-Type Checklist

**Covers:** Health & Fitness, Medical, wellness, nutrition, mental health, medication apps, HealthKit users, health research apps.

## The three health regimes

1. **Consumer wellness/fitness** (workouts, habits, sleep): normal guidelines + privacy sensitivities.
2. **Medical claims** (diagnosis, treatment, measurement): 1.4.1 scrutiny — accuracy data/methodology or rejection; regulatory clearance docs on request.
3. **Health research** (human subjects): 5.1.3 — consent + ethics-board approval.

## Top rejection reasons

| Risk | Guideline | Detail |
|---|---|---|
| Sensor-based medical measurement claims | 1.4.1 | BP, temperature, blood glucose, SpO₂, "x-ray" from device sensors alone = rejected; disclose methodology for measurement claims |
| Drug dosage calculator from unapproved submitter | 1.4.2 | manufacturer/hospital/university/insurer/pharmacy/FDA-equivalent only |
| No "consult a doctor" framing on medical info | 1.4.1 | remind users; don't position as diagnosis/treatment |
| Individual developer in a regulated health field | 5.1.1(ix) | submit as the legal entity providing the service |
| HealthKit data used for ads/marketing/profiling | 5.1.2(vi), 5.1.3(i) | never; third parties included |
| Writing false data into HealthKit | 5.1.3(ii) | also: no personal health info in iCloud |
| Health research without informed consent / ethics approval | 5.1.3(iii)(iv) | consent must cover purpose/duration, risks, confidentiality/sharing, contact, withdrawal |
| Mental-health AI chatbot marketed as therapy | 1.4.1, 2.3.1 | crisis language, "replaces therapy" claims |
| Eating-disorder-adjacent content | 1.1, 1.4.5 | extreme fasting/promotion patterns |
| Missing specific HealthKit purpose strings | 5.1.1(ii) | "share workouts" not "access health data" |

## Audit specifics

- **Claims inventory:** scan marketing strings + App Store description for diagnose/treat/cure/measure vocabulary; every claim needs disclosed basis (1.4.1) — or soften to wellness framing.
- **HealthKit:** read vs write types enumerated; purpose strings per type; data never crosses into ad/analytics SDKs — verify SDK init doesn't ingest HealthKit store data; `NSHealthShareUsageDescription`/`NSHealthUpdateUsageDescription` present.
- **Research flows:** consent screens match 5.1.3(iii) checklist; ethics approval letter linked in review notes.
- **Nutrition/fasting apps:** age gating for extreme regimes; disclaimers; no promotion of harmful behavior (1.4.5).
- **Medication reminders:** OK as reminders; anything dosage-advisory triggers 1.4.2.
- **Demo data:** reviewers must see realistic populated data — seed demo account with workouts/measurements (2.1).

## Pre-submission checklist

- [ ] Every health/measurement claim has disclosed data + methodology (or is removed)
- [ ] "Not a medical device / consult a professional" framing present
- [ ] HealthKit purpose strings specific; data isolated from ad/analytics SDKs
- [ ] Legal-entity submitter if regulated medical field
- [ ] Consent + ethics docs attached if research
- [ ] Demo account pre-populated with realistic health data
