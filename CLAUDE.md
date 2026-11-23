# learnt.ai: Embed launch marketing

This repository holds the marketing work for a major, multi-channel launch of **Embed**, by Learnt.ai: press, paid ads, search, social, communities and launch sites such as Product Hunt.

## Rule 1: the brief is the source of truth about Embed

Read `docs/embed-brief.md` in full before writing anything. It is the only source of facts, numbers, claims and approved wording about Embed.

- Use only what is in the brief. If something you need isn't there, don't invent it: add it to `marketing/flags.md` and leave a placeholder.
- Quote numbers, prices and science claims exactly, with their sources.
- If anything else (this file, an agent definition, research, a platform) disagrees with the brief, the brief wins. Flag the conflict.
- Never edit `docs/embed-brief.md`. Only a human changes it.

## Rule 2: facts about the outside world need a source and a date

Publications, communities, launch sites, ad platforms, search volumes, character limits and platform rules change often. Any fact that isn't about Embed:

- must come from research with a source link and the date it was checked
- is marked `[VERIFY]` if it couldn't be confirmed from the source itself
- lives in `marketing/research/` so everyone uses the same version.

Never present a guess about a publication, audience size, cost or rule as fact.

## Rule 3: the team drafts, humans act

The team plans and writes. It never:

- sends an email or pitch, or contacts a journalist, community or person
- posts, publishes, submits a listing or schedules anything
- buys ads, sets budgets or commits spend
- creates accounts or signs up for anything.

Everything ends as a draft marked `ready for human`.

## Rule 4: earn attention honestly

These apply to every channel, and the brand reviewer enforces them.

- **No fake activity.** No fake accounts, sock puppets, vote rings, bought upvotes, bought reviews or "organic" posts that hide who wrote them.
- **No vote-begging.** On Product Hunt, Hacker News, Reddit and similar, ask for feedback and attention, never for upvotes. Follow each site's own rules, which the researcher records.
- **Always disclose.** Anyone posting about Embed says they're from Learnt.ai.
- **Respect community rules.** Check each group's self-promotion rules before drafting for it, and give value first: a useful post, not a pitch.
- **Ads follow the rules.** UK advertising rules (the CAP Code) require claims to be backed by evidence. The brief's approved claims are the evidence; nothing else is.
- **People's details.** Record only publicly listed, work-related contact routes such as newsdesks, submission forms and published bylines, with a source. No private or personal contact details.

## Placeholders

- `[HUMAN: what is needed and why]` means a person must supply or decide something.
- `[VERIFY: what to check, and where]` means an outside fact must be confirmed before use.

Search for both before anything goes live.

## Known gaps in the brief

Don't fill these: use `[HUMAN: ...]` placeholders. The full list is in `marketing/flags.md`.

- The "Get started free" link, and when to use each call to action.
- The brief's "open questions" section, which isn't included yet.
- Integrations: unconfirmed, so never claim any.
- Launch date, budget, KPI targets, spokespeople, founder names and quotes, and brand assets (logo, screenshots, demo video).

## The team

Teammate definitions live in `.claude/agents/`. Each teammate owns one folder and doesn't write in anyone else's folder.

| Teammate | Owns | Folder |
|---|---|---|
| `launch-strategist` (lead) | Launch plan, phases, KPIs, budget proposal, assigning work, consistency, final hand-off | `marketing/launch-plan.md` |
| `market-researcher` | Facts about the outside world: publications, communities, launch sites, keywords, platform rules and specs, launch timing | `marketing/research/` |
| `copywriter` | Website, landing pages, use-case pages, pricing page | `marketing/web/` |
| `content-marketer` | Launch blog post, email sequences, bylined articles, sales one-pager, newsletter | `marketing/content/` |
| `pr-manager` | Press strategy, media list, press release, pitches, press kit, embargo plan | `marketing/press/` |
| `paid-media-manager` | Paid campaigns: structure, audiences, ad copy, budget split, testing, optimisation | `marketing/paid/` |
| `seo-conversion-specialist` | Keywords, on-page SEO, landing-page conversion, tracking and UTM plan, reporting | `marketing/seo/` |
| `social-media-manager` | Organic social from the company and founders, posting calendar | `marketing/social/` |
| `community-launch-manager` | Product Hunt and other launch sites, groups and communities, launch-day runbook | `marketing/community/` |
| `creative-director` | Visual and video briefs for every channel, ad creative specs, Product Hunt gallery, demo video script | `marketing/creative/` |
| `brand-reviewer` | Checks every draft against the brief, these rules, ad rules and platform rules | `marketing/reviews/` |

Shared files:
- `marketing/flags.md`: the single list of everything that needs a human. Anyone adds; the strategist tidies; only a human resolves.
- `marketing/research/`: the single source for outside facts. Ask the researcher rather than searching separately.

## How work flows

1. The strategist plans and assigns. The researcher answers outside-fact questions.
2. Writers draft. Every draft starts with this header:
   ```
   Purpose:
   Audience: (a row from the brief's audience table)
   Channel / placement:
   Brief sections used:
   Research used: (files in marketing/research/, if any)
   Status: draft
   ```
3. The brand reviewer checks each draft and gives a verdict of **Ready**, **Ready with edits** or **Not ready**.
4. Writers fix and resubmit. Only **Ready** moves a draft to `ready for human`.
5. Status values: `draft`, `in review`, `changes needed`, `ready for human`.

`docs/team-playbook.md` sets out the launch phases and which teammates run in each.

## House style, in short

The brief's "Voice and style" and "Guardrails" sections are the full rules. The ones most often missed:

- British English. No exclamation marks, no emoji, no hype, including in ads and on launch sites.
- "You" and "your people", never "learners" or "users" outside pricing.
- Lead with performance and risk, not training.
- Embed is "capability reinforcement and assurance", never an LMS, course library or e-learning platform.
- Never say Embed is "proven", never give an Embed result as a percentage, never invent customers, quotes, testimonials or results.
- Certifications belong to Bubble, AWS, OpenAI and OpenRouter, not Learnt.ai. Hosting is in the US.
- No competitor names in any copy. Contrast with "courses", "completion records" or "your LMS". Bidding on or targeting competitor brand names is a human decision: don't propose it as part of the plan without flagging it.
- Words to avoid: revolutionary, game-changing, cutting-edge, seamless, leverage, empower, unlock, synergy, journey, best-in-class, AI-powered (as a headline), learners (outside pricing).
