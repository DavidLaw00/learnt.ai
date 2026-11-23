Purpose: The HubSpot set-up for Embed leads: contact, company and deal fields, pipeline stages, and what fills each one.
Audience: Internal (Dave, setting up HubSpot)
Channel / placement: HubSpot CRM
Brief sections used: Audience, Use cases, Calls to action, Security and data (roles)
Research used: none yet. Plan limits are [VERIFY]
Experiment: none (this is the tracking backbone for every experiment)
Launch timing: both
Status: ready for human

# HubSpot set-up for Embed

## How it fits together

- **Contact:** one person who might buy. Holds where they came from (channel, experiment), their outreach and demo status, and whether we may contact them.
- **Company:** their organisation. Holds size, sector, target tier and paid places.
- **Deal:** one buying opportunity, moving through the **Embed pipeline** from demo booked to paid. This gives you a clean funnel report without extra fields.

Codes such as `LND` and `EXP-R1-03` come from `marketing/growth/taxonomy.md`. Use the same codes in Waalaxy campaign names, UTM tags and here, so the numbers join up.

**Never store in HubSpot the people your customers add to Embed.** They're your customers' data, and Learnt.ai is only their Data Processor (see the brief's Security and data section). HubSpot holds the account owner who signed up, and counts.

**Free plan limits.** Free HubSpot plans may cap the number of custom properties and pipelines `[VERIFY: current limits on your plan]`. Create the **P1** fields first. They're the minimum that makes experiments measurable.

---

## Contact properties

### Use HubSpot's built-in fields

First name, Last name, Email, Job title, Company name, Lifecycle stage, Lead status, Contact owner, Original source (filled automatically for website visitors when the HubSpot tracking code is installed), and Marketing contact status.

### Custom fields: create a property group called "Embed growth"

| Priority | Label | Internal name | Type | Options / format | Filled by |
|---|---|---|---|---|---|
| P1 | Audience | `embed_audience` | Dropdown | CEO, LND, RISK, OPS, EMP, OTHER | Waalaxy zap (from campaign name), or by hand after a call |
| P1 | First-touch channel | `embed_first_channel` | Dropdown | LI-OUT, LI-ORG, LI-PAID, G-PAID, PRESS, PH, COMM, EMAIL, WEB, REFERRAL, OTHER | First zap that creates the contact; never overwritten |
| P1 | First-touch experiment | `embed_first_experiment` | Single-line text | e.g. EXP-R1-03 | First zap that creates the contact; never overwritten |
| P1 | Latest experiment | `embed_latest_experiment` | Single-line text | e.g. EXP-R2-01 | Every zap that touches the contact |
| P1 | Outreach account | `embed_outreach_account` | Dropdown | David, Aidan, None | Waalaxy zap |
| P1 | Outreach status | `embed_outreach_status` | Dropdown | Invited, Connected, Replied, Positive, Negative, No response | Waalaxy zap, or by hand |
| P1 | Demo status | `embed_demo_status` | Dropdown | Booked, Held, No-show, Cancelled, Rebooked | TidyCal zaps; "Held" set by hand after the call |
| P1 | Demo date | `embed_demo_date` | Date | | TidyCal zap |
| P1 | Embed sign-up date | `embed_signup_date` | Date | | Bubble zap (`account_created`) |
| P1 | Lawful basis | `embed_lawful_basis` | Dropdown | Legitimate interest (B2B outreach), Consent (form or booking), Contract (customer) | Set by each zap: Waalaxy = legitimate interest; TidyCal and sign-up = consent or contract |
| P1 | Do not contact | `embed_do_not_contact` | Single checkbox | | Opt-out zap, or by hand. Every other zap must check this first |
| P2 | Opt-out date and source | `embed_opt_out_date`, `embed_opt_out_source` | Date; dropdown (LinkedIn, Email, Call, Other) | | Opt-out zap |
| P2 | Use-case interest | `embed_use_cases` | Multiple checkboxes | COMP, HS, CYBER, SALES, LEAD, CULT | Campaign or ad group; refined by hand after a call |
| P2 | Launch timing | `embed_launch_timing` | Dropdown | Pre-launch, Post-launch | Zap that creates the contact (by date) |
| P2 | Round | `embed_round` | Dropdown | R1, R2, R3 … | Zap that creates the contact |
| P2 | First-touch UTMs | `embed_utm_source`, `embed_utm_medium`, `embed_utm_campaign`, `embed_utm_content` | Single-line text ×4 | | Bubble sign-up zap; TidyCal zap if TidyCal passes UTMs `[VERIFY]` |
| P2 | How did you hear about us? | `embed_how_heard` | Single-line text | | TidyCal booking question |
| P2 | Went live date | `embed_live_date` | Date | | Bubble zap (`went_live`) |
| P3 | Embed account ID | `embed_account_id` | Single-line text | Bubble's unique ID for their organisation | Bubble zap. Lets you match events to the right account |
| P3 | Demo notes link | `embed_demo_notes` | Single-line text | Link to notes | By hand |

---

## Company properties

| Priority | Label | Internal name | Type | Options | Filled by |
|---|---|---|---|---|---|
| Built-in | Company name, Company domain, Industry, Number of employees, LinkedIn company page | | | | HubSpot auto-enrichment, zaps, or by hand |
| P1 | Target tier | `embed_target_tier` | Dropdown | A (ideal), B, C, Not a fit | By hand, using the criteria in `marketing/outbound/targeting.md` |
| P2 | Sector focus | `embed_sector` | Dropdown | `[HUMAN: confirm. Target sectors are an open question in the brief]` | By hand |
| P2 | Users added in Embed | `embed_users_count` | Number | | Bubble zap (`users_milestone`). A count only, never names |
| P2 | Paid places | `embed_paid_places` | Number | | Bubble zap (`plan_changed`), or by hand |
| P3 | Plan | `embed_plan` | Dropdown | Free, Paid annual, Paid monthly, Custom | Bubble zap (`plan_changed`) |

---

## Deal pipeline: "Embed"

Rename the default sales pipeline, or create one if your plan allows `[VERIFY]`.

| Stage | Enters when | Set by |
|---|---|---|
| Demo booked | A TidyCal booking arrives | TidyCal zap creates the deal |
| Demo held | You've had the call | By hand after the call |
| Signed up | Bubble `account_created` arrives for a contact with an open deal | Bubble zap |
| Live | Bubble `went_live` arrives | Bubble zap |
| Paid (won) | Bubble `plan_changed` arrives with a paid plan | Bubble zap |
| Nurture | "Not yet"; follow up at 30, 60 or 90 days | By hand |
| Lost | Not a fit, or no response after follow-ups | By hand |

Post-launch sign-ups with no demo get a deal too, created at **Signed up** by the Bubble zap. That way you see both routes: demo first, or straight to sign-up.

**Deal name:** `[Company] – Embed`. **Custom deal properties:** none needed. Report on the associated contact's channel and experiment.

---

## Reports to build first

1. **Deals by stage, broken down by first-touch experiment.** This is your main growth report.
2. **Demos booked per week, by outreach account** (David or Aidan).
3. **Outreach status counts by experiment:** invited, then connected, replied, positive and booked.
4. **Do-not-contact count.** Should only grow, and every zap should respect it.

The weekly-numbers zap (see `automation-architect`) exports these as counts by experiment ID for the growth lead. Never export names.
