# AI Apps — App-Type Checklist

**Covers:** Generative AI tools (text/image/audio/video), chatbots, AI companions, AI assistants, AI-powered utilities (writing, coding, resume), model playgrounds, agent harnesses. Apple treats AI outputs as content the app publishes.

## Top rejection reasons

| Risk | Guideline | Detail |
|---|---|---|
| Unmoderated NSFW generation | 1.1.4, 1.2 | output classifiers/prompt filters required; mature outputs handled per ratings |
| AI chat used primarily for sexual roleplay | 1.2 escalation | companion apps whose actual usage is explicit = removed, moderation notwithstanding |
| Personal data sent to third-party AI without explicit consent | 5.1.2(i) | disclose sharing with third-party AI (OpenAI/Anthropic/Google etc.) and obtain explicit permission |
| Medical/legal/financial advice from an unqualified AI | 1.4.1, 5.x | claims beyond "general information" invite regulated-field treatment |
| Misleading capability marketing | 2.3.1 | "diagnoses", "writes perfect legal contracts" — marketing must match reality |
| Chatbot presenting as human | 1.1.6 | deceptive features; must be clear it's AI |
| AI UGC (shared generations) without 1.2 stack | 1.2 | community galleries of generations = full moderation |
| Subscription to AI service without disclosures | 3.1.2(c) | what the subscription buys (credits/messages per period) |
| Credits that expire | 3.1.1 | purchased credits must not expire |
| Deepfake/face-swap of real people | 5.2.1, 1.1 | rights + impersonation; election/deception contexts are fatal |
| Model downloads executing later behaviors | 2.5.2, 2.5.19 | shipping dormant capabilities via model updates |

## Audit specifics

- **Prompt/output safety:** input filters (blocked categories: CSAE, violence, hate, sexual) + output classifiers; document the vendor (OpenAI moderation, Perspective, self-hosted) in review notes. Reviewers **will** try to make the AI say something objectionable.
- **Consent surfaces:** whenever user text/images/context go to a third-party model: explicit consent + disclosure in privacy policy (5.1.2(i)); no silent prompt harvesting from contacts/photos/location.
- **Purpose strings:** mic (voice input), camera (scan input), photos (import) — all with AI-specific explanations; data-minimization on what context gets sent.
- **Age gating:** AI companions/generative media serving mature outputs → 17+ and age checks; creators-style aggregated AI content → 1.2.1(a) verified/declared age restriction.
- **Companion apps:** the 1.2 escalation pattern applies — if usage skews explicit, structural changes (identity, filters, restricted personas) are required, not more report buttons.
- **Crisis handling:** mental-health-adjacent chatbots need crisis-resource responses (self-harm prompts) — reviewers probe this.

## Pre-submission checklist

- [ ] Input + output moderation wired and documented; reviewer-abuse tested
- [ ] Third-party AI data sharing disclosed + consented (policy + UI)
- [ ] AI identity clear (never passing as human)
- [ ] Mature outputs: off by default, rating aligned, age gates where needed
- [ ] Shared generations = 1.2 stack; community moderation live
- [ ] Credits/subscriptions: IAP, no expiry, full paywall disclosures
- [ ] No regulated-advice claims in marketing or in-model behavior
