Purpose: How Embed billing works in Paddle Billing, and what the Bubble app must do to keep it right.
Audience: Internal (Learnt.ai team building the Bubble app and Paddle setup)
Channel / placement: Paddle Billing (sandbox first), Bubble app back end
Brief sections used: Pricing (bands, worked examples, monthly 25% uplift)
Research used: none
Experiment: none
Launch timing: both
Status: draft

# Embed billing in Paddle

## Decisions taken (by David Law, in chat, 27 November 2026)

- Prices **exclude** tax. Paddle is merchant of record and adds sales tax or VAT where it applies.
- The first 25 places are free on **both** annual and monthly payment.
- An organisation can import as many people as it likes. It pays only for **people with one or more active capabilities assigned** ("billable places").
- Monthly payment is charged as **one calculated total** (option A), so it matches the brief to the penny. Annual payment uses catalogue band prices.
- When billable places go up mid-term, the difference is **charged straight away, pro rata** (`proration_billing_mode: prorated_immediately`).

[HUMAN: the first two decisions answer two of the brief's "Open questions" (VAT; monthly payment and the free 25). Move the answers into `docs/embed-brief.md` so marketing copy can use them.]

## Sandbox catalogue (built 27 November 2026)

Product: **Embed** `pro_01m8eewgcv6etnh0t1gcezq44p`, tax category `saas`.

| Price | ID | Unit price | Cycle | Quantity range | Tax mode |
|---|---|---|---|---|---|
| Places 26 to 250, annual | `pri_01m8eewge0946hf2kef2evgkx8` | £48.00 | yearly | 1 to 225 | external (excl. tax) |
| Places 251 to 500, annual | `pri_01m8eewgez4wqyk5rs2s9vese6` | £42.00 | yearly | 1 to 250 | external (excl. tax) |
| Places 501 to 1,000, annual | `pri_01m8eewgg0mtpsmae3hsd259hx` | £36.00 | yearly | 1 to 500 | external (excl. tax) |

There is no monthly catalogue price. Monthly subscriptions carry one non-catalogue price created per organisation (see below). Over 1,000 places is a custom price agreed per deal, so it has no catalogue price either.

These are **sandbox** IDs. Live IDs will differ once the catalogue is copied to production.

## How the app works out the bill

Let `n` = billable places (people with one or more active capabilities).

Places in each band:

- band 26–250: `max(0, min(n, 250) − 25)`
- band 251–500: `max(0, min(n, 500) − 250)`
- band 501–1,000: `max(0, min(n, 1000) − 500)`
- `n` over 1,000: stop and route to sales for a custom price.

**Annual:** one subscription item per band with a quantity above 0, each using that band's price ID above. Drop an item when its band quantity reaches 0.

**Monthly:** one item, quantity 1, with a non-catalogue price:

- amount (pence) = `round(band1 × 500 + band2 × 437.5 + band3 × 375)`
- billing cycle: monthly; tax mode: `external`; product: the Embed product
- name, for invoices: "N paid places, monthly"

**When `n` = 25 or fewer:** no paid items. [HUMAN: decide whether free organisations get a Paddle subscription at all (a £0 subscription, for example) or only when they first pass 25.]

## Checked totals (sandbox previews, GB address, 27 November 2026)

| Billable places | Annual, excl. tax | Monthly, excl. tax | Brief says |
|---|---|---|---|
| 100 | £3,600.00 | £375.00 | £3,600 / £375.00 |
| 300 | £12,900.00 | £1,343.75 | £12,900 / £1,343.75 |
| 1,000 | £39,300.00 | £4,093.75 | £39,300 / £4,093.75 |

Paddle added UK VAT at 20% on top for a GB buyer with no VAT number. Tax for other buyers is Paddle's job.

## Organisations

| Embed (Bubble app) | Paddle |
|---|---|
| Organisation | Customer (billing contact's email) + Business (organisation name, VAT or tax number, if any) + Address |
| Organisation ID | `custom_data.org_id` on the customer and on the subscription |
| Billable places | Subscription item quantities (annual) or the calculated monthly price |

1. **Upgrade (first paid place):** the app creates or finds the Paddle customer, then opens Paddle checkout with the items worked out above. The organisation ID goes in `custom_data.org_id`.
2. **Capability assigned or removed:** recalculate `n`. If the result differs from what's billed, update the subscription:
   - going up: charge pro rata straight away
   - going down: [HUMAN: decide: credit straight away, or apply at renewal. A common choice is to apply decreases at the next renewal to avoid credit churn.]
3. **Webhooks:** listen for `subscription.created`, `subscription.updated`, `subscription.canceled`, `transaction.completed` and `transaction.payment_failed`, and store the subscription status on the organisation.
4. **Billing self-service:** use Paddle's customer portal (one portal session per request) for invoices, payment method and cancellation.

[HUMAN: decide whether one change of `n` updates Paddle immediately, or whether changes are batched (for example once a day) to avoid many small pro-rata charges when an organisation assigns capabilities in bulk.]

## Still to do

- [HUMAN: webhook endpoint URL in the Bubble app, then create the notification setting in sandbox and test with Paddle simulations]
- [HUMAN: client-side token for Paddle.js checkout in Bubble (sandbox token starts `test_`)]
- [HUMAN: approve copying the catalogue to live; a person switches it on]
- [HUMAN: currencies other than GBP, if wanted. None are set up yet.]

ready for human: no (draft)
