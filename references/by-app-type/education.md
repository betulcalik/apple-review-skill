# Education — App-Type Checklist

**Covers:** Education apps, classroom tools, study aids, language learning, ClassKit/ Schoolwork integrations, student-facing apps.

## Top rejection reasons

| Risk | Guideline | Detail |
|---|---|---|
| Requiring students to sign in with a personal social account | 4.8 (exemption exists) | education apps requiring an existing **education/enterprise account** are exempt from SIWA — personal social logins are not |
| Student/classroom data used for ads or profiling | 5.1.2(vi) | ClassKit data: never for marketing/ads/mining |
| "For Kids/Children" marketing while not in Kids Category | 2.3.8, 5.1.4(b) | audience-implying metadata is reserved for Kids Category |
| Kids-level restrictions ignored for apps actually used by minors | 1.3, 5.1.4 | if primary audience is children, Kids rules apply regardless of category choice |
| Classroom app without working demo class/student data | 2.1 | reviewer needs populated demo: demo account with real-looking classes, assignments, grades |
| School/enterprise purchases sold as consumer IAP | 3.1.3(c) | direct-to-institution offerings may bypass IAP; consumer single-user sales may not |
| Vague subscription for course access | 3.1.2(c) | what content, how much, how often — paywall disclosures |

## Audit specifics

- **Login model:** who authenticates? District SSO / edu accounts → document in review notes (4.8 exemption). Provide demo credentials that exhibit a realistic classroom (roster, content, progress).
- **ClassKit/Student data:** `ClassKit` APIs — data never leaves education context; no ad SDKs touching it; purpose strings education-specific.
- **Age spread:** education apps often serve minors — decide and document: is this Kids Category, kids-primary (1.3/5.1.4 constraints), or 13+ general audience with age screening? Metadata and data practices must match that story.
- **Discounted edu offerings:** volume purchase / custom apps via Apple School Manager → different distribution, not IAP rules; consumer version separately.
- **Homework-help/AI tutors:** AI-generated answers for minors → parental expectations, content filters (see `ai-apps.md`), and honest marketing of what the AI does (2.3.1).

## Pre-submission checklist

- [ ] Demo account shows a populated classroom/course experience
- [ ] Login scheme documented for review (edu SSO exemption claim is explicit)
- [ ] Student data flows audited: no ads/analytics/profiling
- [ ] Audience decision made (Kids vs kids-primary vs 13+) and metadata aligned
- [ ] Course/subscription paywalls disclose content quantity and cadence
