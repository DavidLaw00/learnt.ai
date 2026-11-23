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

## Open questions and known gaps

**The brief's "Open questions" section is off limits.** Nothing listed there may be used, implied or worked around until a human answers it and moves the answer into the brief. The ones most likely to catch you out:

- **VAT.** Never say whether prices include VAT.
- **Monthly payment and the free 25.** Don't say whether the first 25 places are free when paying monthly. Say "First 25 users free" only in the brief's own wording.
- **Integrations, and how people receive nudges.** Don't name email, Slack, Teams, an app or any LMS or HR system as a delivery route or integration.
- **Customer proof.** None exists.
- **Target size and sectors.** Not set. Plans may propose a focus, marked `[HUMAN: confirm]`, but copy must not name one.
- **Walkthrough film.** Don't describe its length or content.
- **Social handles.** Use `[HUMAN: official handle]`.
- **Sign-off.** Who approves agent-written content isn't decided. Every draft ends as `ready for human`.
- **The tool library closure.** Never announce it or give a date. Say "we've put our focus on Embed".
- **Founders' LinkedIn profiles.** They are out of date, and Aidan's says "free" and "around 30 minutes". Don't quote or link to them as a source. Any plan that sends people to the founders' profiles (founder posts, Product Hunt maker profiles, press bios) must flag that the profiles should be updated first.

**Not in the brief at all.** Use `[HUMAN: ...]`. The full list is in `marketing/flags.md`.

- Launch date, budget, KPI targets and target markets.
- Quotes from David Law or Aidan Brooks. The team may draft a suggested quote from the brief's approved lines, clearly labelled "draft quote for approval".
- Brand assets: logo, colours, fonts and screenshots.
- A media contact other than hello@learnt.ai, if one is wanted.

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

The brief's "Vision", "Our story", "Hopes and fears", "Voice and style", "Guardrails" and "Names, links and company details" sections are the full rules. The ones most often missed:

- **Names.** Write "Embed by Learnt.ai" on first mention where both are needed. Learnt.ai has a capital L and a lower-case ".ai". Embed has a capital E. Never promote Create.
- **Calls to action.** "Get started free" (https://learnt.ai/signup) always comes first. "Book a demo" is only ever the secondary option. Use only links from the brief's tables.
- **Vision and story.** Use them for the why (About, founder posts, press, launch sites, openings of long pieces), not on product or use-case pages. Story numbers are exact: "a combined 35+ years in people development", "110 AI tools", "more than 34,000 users". Don't split the 35+ years between the founders. The only past employers that may be named are those in the brief's founders table. Never use the words "passionate" or "authentic".
- **AI fears.** Name a fear only to answer it with a hope and an action, and never end on fear. No job-loss predictions, no AI statistics or forecasts, never "AI will replace people".
- **The AI Community of Practice.** Mention it only to show Learnt.ai is part of the conversation (learnt.ai/join-the-community). Never use it, or any post in it, to sell or launch Embed.
- **Personal details.** Founders' personal phone numbers or emails are never used. Use hello@learnt.ai.
- British English. No exclamation marks, no emoji, no hype, including in ads and on launch sites.
- "You" and "your people", never "learners" or "users" outside pricing.
- Lead with human capability, performance and proof. AI sets the stakes; Embed is how people keep up. Not training.
- Embed is "capability reinforcement and assurance", never an LMS, course library or e-learning platform.
- Never say Embed is "proven", never give an Embed result as a percentage, never invent customers, quotes, testimonials or results.
- Certifications belong to Bubble, AWS, OpenAI and OpenRouter, not Learnt.ai. Hosting is in the US.
- No competitor names in any copy. Contrast with "courses", "completion records" or "your LMS". Bidding on or targeting competitor brand names is a human decision: don't propose it as part of the plan without flagging it.
- Words to avoid: revolutionary, game-changing, cutting-edge, seamless, leverage, empower, unlock, synergy, journey, best-in-class, AI-powered (as a headline), learners (outside pricing).
