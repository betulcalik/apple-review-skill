# apple-review-skill

An AI agent skill that audits apps against **everything Apple's App Review has ever rejected apps for** — every rule in the App Store Review Guidelines (current through the **October 16, 2026** revision), the upload-validation gate (ITMS errors), App Store Connect metadata, and the post-rejection process — before Apple does.

Built to be run by coding agents (pi, Claude Code, Codex CLI, Cursor, any agent that reads markdown skills). Stack-agnostic: native Swift/ObjC, React Native/Expo, Flutter, hybrid.

## Why

~25% of App Store submissions are rejected (1.93M of 7.77M in Apple's 2024 transparency report), and the top categories — Performance, Legal, Design, Business, Safety — are dominated by **preventable, mechanical mistakes**: a missing report button, a vague purpose string, a dead demo account, a paywall without disclosures, a `UserDefaults` call without a privacy-manifest reason.

This skill encodes:

- **All five guideline sections, every rule** (1.1–5.6.4 incl. 2026 additions: creator age gating 1.2.1(a), creative assets 2.3.14, hidden features 2.5.19, revised external-purchase-link rules 3.1.1(a))
- **A social-media / UGC deep dive** — 1.2 is the single most common social-app rejection and Apple checks it as a literal four-item checklist (filter · report · block · contact). Includes the fatal "used primarily for objectionable content" patterns, anonymous-chat bans, NSFW toggle rules, age ratings, forced-login rules, and social monetization (coins/boosts/tips)
- **A rejection case database** — real rejection messages keyed by guideline, classified by response type (binary / metadata / information-needed), with the fixes that resolved them
- **The upload gate** — ITMS-91053 privacy manifests, 90809 deprecated APIs, 90683 purpose strings, signing and icon families, plus a pre-upload sweep
- **An evidence-based audit workflow** — what to grep, which files to read, severity levels (P0–P3), and a rule against claiming a rejection risk without evidence in the repo
- **Rejection-response playbook** — Resolution Center reply structure, appeals, expedited reviews, bug-fix submissions

## Install

Any agent that supports markdown skills can use this repo. Point your agent at it (clone, submodule, or package):

```bash
git clone https://github.com/betulcalik/apple-review-skill.git
```

- **pi:** place/symlink the folder under `~/.pi/agent/skills/apple-review/`
- **Claude Code:** copy to `.claude/skills/apple-review/` (project) or `~/.claude/skills/` (user)
- **Codex / Cursor / others:** put `SKILL.md` + `references/` wherever your agent loads instructions from and reference it in your AGENTS.md/rules file

## Use

Ask your agent, in the app's repo:

- "Audit this app for App Store rejection risks" → full pre-submission audit
- "Apple rejected us: *<paste rejection message>*" → triage + fix plan + draft reply
- "Does boosting posts require IAP?" → rule lookup with the exact guideline citation

## Repo layout

```
SKILL.md                        # agent entry: workflow, severity, report format
references/
  00-preflight.md               # discovery methodology + "Before You Submit" mapping
  01-safety.md                  # 1.1–1.7 incl. full 1.2 UGC rules
  02-performance.md             # 2.1–2.5.19 completeness, metadata, software rules
  03-business.md                # 3.1–3.2.2 IAP, subscriptions, exceptions, crypto
  04-design.md                  # 4.1–4.10 copycats, spam, login, extensions
  05-legal.md                   # 5.1–5.6.4 privacy, IP, gambling, VPN, conduct
  social-media.md               # deep dive: UGC/social app audit
  rejection-cases.md            # real rejection messages + fixes, by guideline
  upload-errors.md              # ITMS-* upload validation + pre-upload sweep
  appeals.md                    # Resolution Center, appeals, expedite playbook
```

## Method

The skill drives a structured audit: detect the stack → inventory risk surfaces (Info.plist, entitlements, privacy manifests, SDKs, money flows, UGC surfaces) → walk the five guideline sections → check the upload gate → emit a severity-ranked report where **every finding cites a rule number, repo evidence, and a concrete fix**. Post-rejection mode matches Apple's message against the case database and drafts the response. Findings without evidence are labeled `NEEDS-VERIFY` instead of guessed.

## Sources & credits

- Apple's [App Review Guidelines](https://developer.apple.com/app-store/review/guidelines/) and [App Review news updates](https://developer.apple.com/news/) — rules paraphrased/summarized, current through the 2026-10-16 revision
- Apple's [App Store Transparency Reports](https://www.apple.com/legal/app-store/transparency/)
- Public documentation of real rejections (appstorereject.com, developer forums, Stack Overflow) — rejection message shapes and fixes
- Prior art studied for structure: safaiyeh/app-store-review-skill, cruisediary/apple-app-review-skills, UppercutLabs/heimdall — this repo aims to be the most complete and best-organized of them

**Not legal advice.** Guideline summaries are informational; regulated categories (gambling, health, finance, kids' privacy) need professional review.

## Maintenance

Apple revises the guidelines several times a year. Each reference file states what it covers; check [developer.apple.com/news](https://developer.apple.com/news/) for guideline updates before trusting any finding near a submission date. PRs welcome — especially new real-world rejection cases (paste the rejection text + what fixed it).

## License

MIT — see [LICENSE](LICENSE).
