# Embed launch: team playbook

How to run the eleven-person marketing team. Launch day is **L**; dates are relative until you set one.

## Why phases

An agent team works best with a lead and **three to five active teammates** at once. With more than that, coordination costs more than it saves. So the team runs in four phases, each with only the teammates whose inputs are ready. Everything they write stays in `marketing/`, so each phase builds on the last.

## Before you start: answer the blocking questions

Open `marketing/flags.md` and answer what you can under **Blocking**, especially the launch date, budget range and sign-off. Then work through the brief's **Open questions** in `docs/embed-brief.md`: answer each one by moving the answer into the brief. Every answer removes placeholders from every draft.

**When you update the brief:** edit `docs/embed-brief.md` (only humans change it) and commit. Teammates read it fresh each time they start, so the next phase picks up the change.

---

## Phase 1: Foundation (L-6 to L-4 weeks)

**Who:** `launch-strategist` (lead), `market-researcher`, `seo-conversion-specialist`, `creative-director`

**Produces:**
- the launch plan, message house, story angles, channel plan, budget split and KPIs
- research on publications, communities, launch sites, keywords, ad platforms and the calendar
- the keyword map and the tracking and UTM plan
- the visual direction and the asset list

**Prompt:**
```
Create an agent team for Phase 1 of the Embed launch, as described in docs/team-playbook.md. Use launch-strategist as lead, with teammates market-researcher, seo-conversion-specialist and creative-director, using their definitions in .claude/agents. Start with market-researcher on publications, communities, launch sites, keywords, ad platforms and the calendar, while launch-strategist drafts the launch plan and message house. When research lands, have seo-conversion-specialist build the keyword map and tracking plan, and creative-director write the visual direction and asset list. Finish with the strategist's phase summary and an updated marketing/flags.md.
```

**Your review:** read `marketing/launch-plan.md` and approve the channel mix, budget split, story angles and launch date, then answer any new flags.

---

## Phase 2: Build the core (L-4 to L-2 weeks)

**Who:** `launch-strategist` (lead), `copywriter`, `content-marketer`, `pr-manager`, `brand-reviewer`

**Produces:**
- the website and landing pages, science and security pages, and pricing
- the launch blog post, supporting posts, email sequences and one-pager
- the press strategy, media list, press release, pitches, byline proposals and press kit
- reviews of all of the above

**Prompt:**
```
Create an agent team for Phase 2 of the Embed launch, as described in docs/team-playbook.md. Use launch-strategist as lead, with copywriter, content-marketer, pr-manager and brand-reviewer from .claude/agents. Build on everything in marketing/ from Phase 1. The writers draft in parallel; brand-reviewer reviews each piece as it's marked in review, and writers fix until it's Ready. Finish with the strategist's phase summary and an updated marketing/flags.md.
```

**Your review:** approve the press release and pitches, add the founder quote and bio, decide on any exclusive or embargo, and start the human actions on the press timeline.

---

## Phase 3: Channels (L-3 to L-1 weeks)

**Who:** `launch-strategist` (lead), `paid-media-manager`, `community-launch-manager`, `social-media-manager`, `brand-reviewer`

`creative-director` can join for a final asset pass. Run it as a short follow-up team if you want to keep within five.

**Produces:**
- campaign plans, ad copy, keywords, the testing and optimisation rulebook
- the Product Hunt listing, maker comment, gallery plan, prep checklist, launch-day runbook and reply guide
- other launch-site listings
- community posts written to give value first
- founder and company social posts, and the calendar
- reviews of all of the above

**Prompt:**
```
Create an agent team for Phase 3 of the Embed launch, as described in docs/team-playbook.md. Use launch-strategist as lead, with paid-media-manager, community-launch-manager, social-media-manager and brand-reviewer from .claude/agents. Build on everything in marketing/. Line up the ads, Product Hunt day, community posts and social calendar with the press timeline in the launch plan. brand-reviewer checks everything, especially the honest-attention rules. Finish with the strategist's launch-readiness summary and an updated marketing/flags.md.
```

**Your review:** set up the ad accounts, tracking and Product Hunt accounts; approve the founder posts; confirm who answers replies on launch day.

---

## Phase 4: Launch and after (L-1 to L+4 weeks)

**Who:** `launch-strategist` (lead), `seo-conversion-specialist`, `paid-media-manager`, `content-marketer` or `pr-manager` as needed, `brand-reviewer`

The team can't see live data. Paste or upload results (ad reports, analytics exports, Product Hunt stats, coverage links), and it will analyse them and propose next steps.

**Prompt (repeat weekly):**
```
Create an agent team for Phase 4 of the Embed launch, as described in docs/team-playbook.md. Use launch-strategist as lead, with seo-conversion-specialist and paid-media-manager, plus brand-reviewer for any new copy. Here are this week's results: [paste or attach]. Analyse them against the KPIs in the launch plan, apply the optimisation rulebook, and propose budget moves, new ad variants and follow-up content and press. Don't present results as Embed outcomes in any marketing copy without a human's approval.
```

---

## What the team will never do

- Send, post, submit, buy or sign up for anything. You do every outward action.
- Invent customers, quotes, results, statistics or facts about Embed.
- Ask for upvotes, use fake accounts, or post without disclosing who it's from.

See `CLAUDE.md` for the full rules.

## Running a single teammate

For a one-off job, you don't need a team. Ask for one subagent, for example: "Use the brand-reviewer agent to review marketing/web/home.md."
