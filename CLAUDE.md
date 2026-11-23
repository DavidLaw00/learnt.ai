# learnt.ai: Embed launch marketing

This repository holds the marketing work for the launch of **Embed**, by Learnt.ai.

## The brief is the source of truth

Read `docs/embed-brief.md` in full before writing anything. It is the only source of facts, numbers, claims and approved wording about Embed.

- Use only what is in the brief. If something you need isn't there, don't invent it: add it to `marketing/flags.md` and leave a placeholder in the draft.
- Quote numbers, prices and science claims exactly, with their sources.
- If anything else (this file, an agent definition, another document) disagrees with the brief, the brief wins. Flag the conflict.
- Never edit `docs/embed-brief.md`. Only a human changes it.

## Placeholders

Mark anything a human has to supply or confirm like this, so it is easy to search for:

`[HUMAN: what is needed and why]`

## Known gaps in the brief

These are not yet in the brief. Don't fill them; use a `[HUMAN: ...]` placeholder.

- **"Get started free" link.** The calls-to-action table gives no link for the main button.
- **When to use each call to action.** The "When" column is empty.
- **Open questions.** The brief refers to an open questions section, but none is included yet.
- **Integrations.** Unconfirmed: never claim any.
- **Launch details.** Launch date, launch type and channels haven't been set. The launch strategist proposes them in `marketing/launch-plan.md`, marked as proposals for a human to confirm.

## The team

Teammate definitions live in `.claude/agents/`:

| Teammate | Owns | Writes to |
|---|---|---|
| `launch-strategist` (lead) | Launch plan, channel plan, timeline, assigning work, final sign-off list | `marketing/launch-plan.md` |
| `copywriter` | Website, landing page and use-case page copy, ads | `marketing/web/` |
| `content-marketer` | Launch blog post, email sequence, press release, sales one-pager | `marketing/content/` |
| `social-media-manager` | LinkedIn and other social posts, posting calendar | `marketing/social/` |
| `brand-reviewer` | Checks every draft against the brief before it is marked ready | `marketing/reviews/` |

Shared file: `marketing/flags.md` is the single list of everything that needs a human. Any teammate can add to it; never delete someone else's entry.

## How work flows

1. The strategist writes the launch plan and splits it into tasks.
2. Writers draft. Each draft starts with a short header: purpose, audience (from the brief's audience table), channel, status.
3. The brand reviewer checks each draft and records a verdict: **Ready**, **Ready with edits** or **Not ready**, with reasons.
4. Writers fix anything marked not ready. Nothing is final until the reviewer passes it.
5. Status values in draft headers: `draft`, `in review`, `changes needed`, `ready for human`.

Nothing is published by the team. Every piece ends with a human.

## House style, in short

The brief's "Voice and style" and "Guardrails" sections are the full rules. The ones most often missed:

- British English. No exclamation marks, no emoji, no hype.
- "You" and "your people", never "learners" or "users" outside pricing.
- Lead with performance and risk, not training.
- Embed is "capability reinforcement and assurance", never an LMS, course library or e-learning platform.
- Never say Embed is "proven", never give an Embed result as a percentage, never invent customers, quotes or testimonials.
- Certifications belong to Bubble, AWS, OpenAI and OpenRouter, not Learnt.ai. Hosting is in the US.
- No competitor names. Contrast with "courses", "completion records" or "your LMS".
- Words to avoid: revolutionary, game-changing, cutting-edge, seamless, leverage, empower, unlock, synergy, journey, best-in-class, AI-powered (as a headline), learners (outside pricing).
