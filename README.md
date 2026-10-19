<div align="center">

<a href="https://github.com/betulcalik/apple-review-skill">
  <img src="docs/assets/banner.svg?v=3" alt="apple-review-skill" width="860" />
</a>

[![license](https://img.shields.io/badge/license-MIT-22c55e)](./LICENSE)
[![guidelines](https://img.shields.io/badge/App%20Review%20Guidelines-2026--10--16-60a5fa)](https://developer.apple.com/app-store/review/guidelines/)
[![format](https://img.shields.io/badge/format-agent%20skill%20(SKILL.md)-fbbf24)](./SKILL.md)
[![scope](https://img.shields.io/badge/coverage-Guidelines%201%E2%80%935%20%C2%B7%20UGC%20%C2%B7%20ITMS%20%C2%B7%20appeals-c084fc)](#what-it-covers)
[![platforms](https://img.shields.io/badge/stacks-Swift%20%C2%B7%20RN%2FExpo%20%C2%B7%20Flutter-8b949e)](#install)

### An AI agent skill that audits your app against **everything Apple's App Review has ever rejected apps for** — and tells you exactly what to fix.

**Every guideline 1.1 → 5.6.4 · UGC/social deep dive · real rejection messages · ITMS upload gate · appeals playbook**

[Install](#install) · [Quick start](#quick-start) · [Why](#why) · [What it covers](#what-it-covers) · [How it works](#how-it-works) · [Repo layout](#repo-layout) · [Sources](#sources--credits)

</div>

> [!IMPORTANT]
> **This is an AI agent skill — not an official Apple tool, and not a guarantee of approval.**
> It is an unofficial, community-maintained checklist-and-workflow that an AI agent (pi, Claude Code, Codex, Cursor…) reads to audit your project. Its findings are AI-generated assessments based on the public [App Review Guidelines](https://developer.apple.com/app-store/review/guidelines/) — they can be wrong, stale, or incomplete. Apple changes the rules several times a year and applies them with human judgment. Nothing here is affiliated with or endorsed by Apple, and nothing here is legal advice. Always verify against the official guidelines before submitting — and for regulated categories (gambling, health, finance, kids' privacy), talk to a lawyer.

## Why

~25% of App Store submissions get rejected (1.93M of 7.77M in Apple's 2024 transparency report), and the volume leaders — Performance, Legal, Design, Business, Safety — are dominated by **preventable, mechanical mistakes**: a missing report button (1.2), a vague purpose string (5.1.1), a dead demo account (2.1), a paywall without disclosures (3.1.2), a `UserDefaults` call without a privacy-manifest reason (ITMS-91053).

Reading the guidelines is not enough — you need to check your *actual project* against them, rule by rule, with evidence.

| Without this skill | With it |
| --- | --- |
| You re-read 130+ rule subsections before every submit | Agent walks all five sections against your repo automatically |
| Social app bounces on 1.2 — one of the four features missing | 1.2 checked as Apple's literal checklist, with the review-notes paragraph pre-drafted |
| Rejection email arrives → you guess what Apple meant | Message matched against real documented rejections + cheapest response path (binary vs metadata vs info-needed) |
| Upload bounces on ITMS-91053 after review wait | Pre-upload sweep catches manifest/purpose-string/deprecation issues first |
| You argue in Resolution Center, wait days | Structured reply template that answers every question reviewers ask |

## What it covers

| Area | Contents |
| --- | --- |
| **Guidelines 1–5, every rule** | 1.1–1.7 Safety · 2.1–2.5.19 Performance · 3.1–3.2.2 Business · 4.1–4.10 Design · 5.1–5.6.4 Legal — including the Oct 2026 additions: creator age gating `1.2.1(a)`, creative assets `2.3.14`, hidden features `2.5.19`, revised external-purchase-link rules `3.1.1(a)` |
| **Social media / UGC deep dive** | The 1.2 four-feature checklist (filter · report · block · contact) with implementation bars, the fatal "used primarily for objectionable content" patterns (Chatroulette, anonymous chat, hot-or-not), NSFW web-toggle carve-out, age ratings, forced-login rules, contact-upload rules, social monetization (coins/boosts/tips), push rules |
| **Rejection case database** | Real rejection message shapes keyed by guideline, classified by response type (binary / metadata / information-needed / removal), with the fixes that resolved them |
| **Upload gate** | ITMS-91053 privacy manifests, 90809 deprecated APIs, 90683 purpose strings, signing & icon families + a pre-upload sweep |
| **Rejection-response playbook** | Resolution Center reply structure & templates, App Review Board appeals, expedited reviews, bug-fix submissions |

## How it works

```text
        your app repo
             │
             ▼
   ┌──────────────────┐    detect stack (Swift / RN / Expo / Flutter)
   │  risk inventory  │    Info.plist · entitlements · privacy manifests ·
   │    (Step 1)      │    SDKs · money flows · UGC surfaces
   └────────┬─────────┘
             ▼
   ┌──────────────────┐    every rule → PASS / finding / NEEDS-VERIFY
   │ guideline audit  │    no finding without repo evidence
   │    (Steps 2–5)   │    P0 blocker → P3 polish
   └────────┬─────────┘
             ▼
   ┌──────────────────┐    upload-validation sweep before any human sees it
   │   ITMS gate      │
   └────────┬─────────┘
             ▼
   ┌──────────────────┐    severity-ranked report: rule citation · evidence
   │     report       │    (file:line) · why Apple rejects it · concrete fix
   └──────────────────┘
```

Post-rejection mode: paste Apple's message → match against the case database → classify the response type → fix plan + drafted Resolution Center reply.

## Install

Any agent that reads markdown can use it:

```bash
git clone https://github.com/betulcalik/apple-review-skill.git
```

- **pi:** place/symlink under `~/.pi/agent/skills/apple-review/`
- **Claude Code:** `.claude/skills/apple-review/` (project) or `~/.claude/skills/` (user)
- **Codex / Cursor / other:** put `SKILL.md` + `references/` wherever your agent loads instructions from, and reference it from your `AGENTS.md` / rules file

## Quick start

In your app's repo, ask your agent:

```text
Audit this app for App Store rejection risks.
```

```text
Apple rejected us:
"Guideline 1.2 - Safety - User-Generated Content
 Your app enables the display of user-generated content but does not
 include the required features to moderate this content..."
```

```text
Does selling post boosts in our social app require IAP?
```

Each answer comes back with guideline citations, evidence from your repo, and concrete fixes — see the [report format](./SKILL.md).

## Repo layout

```
SKILL.md                        # agent entry: workflow, severity model, report format
references/
  00-preflight.md               # discovery methodology + "Before You Submit" mapping
  01-safety.md                  # 1.1–1.7 incl. full 1.2 UGC rules
  02-performance.md             # 2.1–2.5.19 completeness, metadata, software rules
  03-business.md                # 3.1–3.2.2 IAP, subscriptions, exceptions, crypto
  04-design.md                  # 4.1–4.10 copycats, spam, login, extensions
  05-legal.md                   # 5.1–5.6.4 privacy, IP, gambling, VPN, conduct
  social-media.md               # deep dive: UGC / social app audit
  rejection-cases.md            # real rejection messages + fixes, by guideline
  upload-errors.md              # ITMS-* upload validation + pre-upload sweep
  appeals.md                    # Resolution Center, appeals, expedite playbook
```

## Sources & credits

- Apple's [App Review Guidelines](https://developer.apple.com/app-store/review/guidelines/) and [news updates](https://developer.apple.com/news/) — paraphrased and summarized, current through the 2026-10-16 revision
- Apple's [App Store Transparency Reports](https://www.apple.com/legal/app-store/transparency/)
- Public documentation of real rejections (appstorereject.com, Apple Developer Forums, Stack Overflow)
- Prior art studied for structure: [safaiyeh/app-store-review-skill](https://github.com/safaiyeh/app-store-review-skill) · [cruisediary/apple-app-review-skills](https://github.com/cruisediary/apple-app-review-skills) · [UppercutLabs/heimdall](https://github.com/UppercutLabs/heimdall) — this repo aims to be the most complete and best-organized of them

**Maintenance:** Apple revises the guidelines several times a year. Check [developer.apple.com/news](https://developer.apple.com/news/) before trusting findings near a submission date. PRs welcome — especially new real-world rejection cases (paste the rejection text + what fixed it).

## License

MIT © Betül Tarhan — see [LICENSE](./LICENSE).
