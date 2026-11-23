# Embed launch: team playbook

How to run the fourteen-person marketing team. Launch day is **L**; dates are relative until you set one.

**The goal:** a calendar full of qualified demos (TidyCal), which become sign-ups after launch, and getting measurably better at it each round.

## Why phases

An agent team works best with a lead and **three to five active teammates** at once. With more than that, coordination costs more than it saves. So the team runs in phases, each with only the teammates whose inputs are ready. Everything they write stays in `marketing/`, so each phase builds on the last.

## Before you start

1. Answer what you can under **Blocking** in `marketing/flags.md`: launch date, budget range, sign-off, CRM choice and your email and chat tools.
2. Work through the brief's **Open questions** in `docs/embed-brief.md`, answering each by moving the answer into the brief.
3. Confirm the **Before launch** note under Calls to action in the brief. It was added from your chat message, so check it says what you mean.

**When you update the brief:** edit `docs/embed-brief.md` (only humans change it) and commit. Teammates read it fresh each time they start.

---

## Phase 1: Foundation (L-6 to L-5 weeks)

**Who:** `launch-strategist` (lead), `market-researcher`, `growth-lead`, `seo-conversion-specialist`

**Produces:**
- the launch plan, message house, story angles and channel plan
- research, including `outreach-tools.md` (Waalaxy, LinkedIn limits, TidyCal, Buffer, Zapier, ICO and PECR)
- the growth taxonomy, funnel, Round 1 experiment backlog and registry
- the tracking and UTM plan

**Prompt:**
```
Create an agent team for Phase 1 of the Embed launch, as described in docs/team-playbook.md. Use launch-strategist as lead, with market-researcher, growth-lead and seo-conversion-specialist from .claude/agents. Start market-researcher on all research files, including outreach-tools.md, while launch-strategist drafts the launch plan and message house. Then growth-lead builds the taxonomy, funnel and Round 1 experiment backlog from the message house, and seo-conversion-specialist builds the keyword map and a tracking plan that uses the taxonomy. Finish with the strategist's phase summary and an updated marketing/flags.md.
```

**Your review:** approve the channel mix, budget, Round 1 experiments and the taxonomy; choose a CRM.

---

## Phase 2: Pipeline engine (L-5 to L-3 weeks; start outbound as early as possible)

**Who:** `launch-strategist` (lead), `outbound-sdr`, `automation-architect`, `growth-lead`, `brand-reviewer`

**Produces:**
- targeting criteria for Waalaxy
- Round 1 LinkedIn sequences and cold email (each a tracked variant)
- reply scripts, the demo playbook and follow-ups
- the CRM design and Zapier specs: opt-outs, TidyCal, Waalaxy, Buffer, alerts, weekly numbers
- compliance notes
- reviews of all of the above

**Prompt:**
```
Create an agent team for Phase 2 of the Embed launch, as described in docs/team-playbook.md. Use launch-strategist as lead, with outbound-sdr, automation-architect, growth-lead and brand-reviewer from .claude/agents. Pre-launch, the call to action is Book a demo. outbound-sdr writes targeting, the Round 1 Waalaxy sequences as defined in growth-lead's registry, reply scripts and the demo playbook. automation-architect writes the CRM design and Zapier specs, starting with opt-out handling and TidyCal bookings. brand-reviewer checks every message and spec. Finish with a build-and-launch checklist for the human and an updated marketing/flags.md.
```

**Your actions:** build the zaps in the order in `marketing/automation/build-order.md` and test them. Set up the Waalaxy campaigns using the experiment IDs as names. Start Round 1 within Waalaxy's safe limits.

---

## Phase 3: Build the core (L-4 to L-2 weeks)

**Who:** `launch-strategist` (lead), `copywriter`, `content-marketer`, `pr-manager`, `brand-reviewer`

**Produces:**
- website pages, in pre-launch (demo) and post-launch (sign-up) versions
- the blog, email sequences, one-pager and customer launch kit
- the press strategy, release, pitches and press kit
- reviews

**Prompt:**
```
Create an agent team for Phase 3 of the Embed launch, as described in docs/team-playbook.md. Use launch-strategist as lead, with copywriter, content-marketer, pr-manager and brand-reviewer from .claude/agents. Build on everything in marketing/, including marketing/growth/learnings.md. The writers draft in parallel, each piece tagged with its experiment ID and launch timing; brand-reviewer reviews until each is Ready. Finish with the strategist's phase summary and an updated marketing/flags.md.
```

---

## Phase 4: Channels (L-3 to L-1 weeks)

**Who:** `launch-strategist` (lead), `paid-media-manager`, `community-launch-manager`, `social-media-manager`, `brand-reviewer`

`creative-director` can run as a short follow-up team for the asset briefs.

**Produces:**
- paid campaigns (demo-led before launch)
- Product Hunt and other launch sites, and community posts
- founder and company posts, the calendar and `buffer-queue.csv` for the approved-row Buffer zap
- reviews

**Prompt:**
```
Create an agent team for Phase 4 of the Embed launch, as described in docs/team-playbook.md. Use launch-strategist as lead, with paid-media-manager, community-launch-manager, social-media-manager and brand-reviewer from .claude/agents. Build on everything in marketing/. Line up ads, Product Hunt day, community posts and the social calendar with the press timeline; pre-launch pieces ask for a demo, launch-day pieces for Get started free. Tag everything with experiment IDs. Finish with a launch-readiness summary and an updated marketing/flags.md.
```

---

## Rounds: launch and every two weeks after

**Who:** `growth-lead` (lead), `outbound-sdr`, `paid-media-manager`, `social-media-manager` or `content-marketer` as needed, `brand-reviewer`

The team can't see your tools. At the end of each round, export **aggregate numbers by experiment ID** from Waalaxy, TidyCal, the CRM, ads, Buffer and analytics, and paste them in. The weekly-numbers zap produces these. Never paste names, emails or profile links.

**Prompt (each round):**
```
Create an agent team to close Round [N] and plan Round [N+1] of the Embed launch, as described in docs/team-playbook.md. Use growth-lead as lead, with outbound-sdr, paid-media-manager, social-media-manager and brand-reviewer from .claude/agents. Here are Round [N] results by experiment ID: [paste aggregates]. growth-lead analyses them against the funnel, records decisions and learnings, and writes the Round [N+1] plan; the others write the new variants it needs; brand-reviewer checks them. Finish with what changed and why.
```

Each round compounds: `marketing/growth/learnings.md` grows into a record of what works for each audience, message and channel.

---

## What the team will never do

- Send, post, submit, buy, sign up for or switch on anything. You run Waalaxy, Zapier, Buffer, TidyCal, the email tool and the CRM.
- Store anyone's personal data in this repository.
- Invent customers, quotes, results, statistics or facts about Embed.
- Ask for upvotes, use fake accounts, or post without disclosing who it's from.

See `CLAUDE.md` for the full rules.

## Running a single teammate

For a one-off job, you don't need a team. Ask for one subagent, for example: "Use the outbound-sdr agent to write three connection-note variants for heads of L&D in compliance."
