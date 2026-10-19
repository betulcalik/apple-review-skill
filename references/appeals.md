# Rejection Response, Resolution Center & Appeals

What to do after Apple says no — the process side. Getting this right saves days; getting it wrong (arguing, resubmitting unchanged, or shipping review-detection tricks) can escalate a routine rejection into an account problem.

## 1. Read the rejection correctly

App Review communications come in three shapes — identify which one you have before doing anything:

| Shape | Signs | What it means |
|---|---|---|
| **Binary rejected** | "…your binary was rejected", Next Steps mention resubmitting a revised binary | you must change the app, upload a new build, resubmit |
| **Metadata rejected** | status shows "Metadata Rejected"; Next Steps say "edit your metadata in App Store Connect" | fix ASC fields only (description, screenshots, age rating, support URL…). No new binary. Resolve the metadata item and reply — review resumes |
| **Information Needed** | "we need additional information to continue the review"; status "Developer Rejected"-adjacent / waiting on reply | answer in Resolution Center. No resubmission needed; review resumes after your reply |

Also check **who** sent it: automated upload emails (ITMS-*) never involve a reviewer — see `upload-errors.md`.

## 2. Triage sequence (matching `rejection-cases.md`)

1. Extract `Guideline X.Y` and the specific missing/violating item from the message (Apple usually pastes the exact requirement).
2. Find the case row; confirm the response type.
3. Decide: fix-and-resubmit (cheapest), clarify-in-reply, or appeal.
4. Draft the reply **before** resubmitting when the rejection is even slightly ambiguous — a good reply can flip "Information Needed" to approved without any code change.

## 3. Writing the Resolution Center reply

Structure that works:

1. **Acknowledge specifically.** "You noted our app was missing a mechanism to report offensive content (Guideline 1.2)."
2. **State the fix concretely with locations.** "Every post, comment, and profile now has a Report option in the '…' menu with reason codes; reports enter a moderation queue we staff 24/7 with a 24-hour SLA."
3. **Make the reviewer's job easy.** Demo account credentials, navigation path to each new feature, video walkthrough link for anything hardware/environment-dependent, documentation for regulated claims.
4. **Answer every question they asked.** Unanswered sub-questions cause a second round-trip.
5. **Tone: factual, cooperative, zero snark.** Never argue the guideline's fairness in the first reply — that's what appeals are for.

**Reply templates:**

- *Missing UGC feature:* "Thank you for the feedback. We have added [feature]. It is accessible via [exact path]. Our moderation process: [SLA/tooling]. Demo account: [credentials] (pre-verified, no OTP)."
- *Information Needed (demo account):* "The demo account is active and excluded from rate limits: [credentials]. If the reviewer encounters any login issue, a fallback fully-featured demo mode is now available from the login screen (approved per 2.1(a)) — tap 'Demo Mode'."
- *Metadata rejected:* "The [field] has been updated in App Store Connect: [what changed]. No changes to the binary."
- *3.1.1 payment flow:* "We have removed the external checkout and replaced it with StoreKit in-app purchase ([product IDs]). Restore Purchases is in Settings."

## 4. Appeals (when the rejection is actually wrong)

- Path: App Review Board via <https://developer.apple.com/contact/app-store/?topic=appeal> or the appeal link in ASC.
- Use when: the rule was misapplied, your feature fits a documented exception, or policy has since changed (cite Apple's own documentation/news pages).
- Include: the guideline cited, why the app complies (with evidence — screenshots, moderation logs, entitlement grants, license docs), and what you ask for.
- For 1.2 "primarily objectionable" removals: moderation logs showing content-action rates and violation prevalence are the documented winning evidence.
- Expect ~1–3 extra days; meanwhile, if a cheap compliant fix exists, ship it anyway — an approved app beats a won argument.

## 5. Process accelerators

- **Expedited review:** <https://developer.apple.com/contact/app-store/?topic=expedite> — critical timing only (event launch, security fix). Abuse gets future requests denied.
- **Bug Fix Submissions:** for an app already on the store, a bug-fix update won't be delayed over unrelated guideline violations except legal/safety issues — tell App Review in Resolution Center you want this path and that violations will be addressed next submission.
- **Phone call:** rejection emails sometimes offer a scheduled reviewer call — take it for ambiguous design/business rejections; they resolve faster verbally.
- **Repeated identical resubmission is the main self-inflicted wound:** same binary + same metadata = same rejection + longer future reviews (Apple states this explicitly). Change something real, or argue with evidence.

## 6. Special situations

- **App removed for objectionable UGC (1.2):** respond with (a) removal of violating content, (b) concrete safeguard upgrades (filters, moderation staffing, age gates), (c) a compliance plan — Apple's text says your response determines whether the app returns.
- **Account termination (Code of Conduct):** written improvement plan is the only restoration route (5.6). Legal counsel is appropriate here.
- **"Not eligible for resubmission":** the account-level quality finding — see 5.6.4; address root causes, not the symptom app.
- **Scam/fraud accusation (3.1.2, 3.1.1(a)):** do not appeal without hard evidence; these carry removal risk.

## 7. After approval

- Ship parity: release what reviewers saw — changing server-side behavior post-approval (feature flags flipping the experience) is the classic 2.3.1(a)/2.5.19/3.1.1 trap.
- Keep rejection history: same-guideline repeat rejections lengthen future reviews.
- Re-run this skill's audit before every significant update; guidelines shift several times a year (last major revision: October 16, 2026).
