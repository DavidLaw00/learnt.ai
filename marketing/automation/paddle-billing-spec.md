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
- When billable places go down, the decrease **applies at renewal**: no credit or refund mid-term.
- The app updates Paddle **once a day**, not on every change.
- An organisation has a Paddle subscription **only when it pays**: 25 or fewer billable places means no subscription. At 26 places it pays £48 a year (annual) or £5.00 a month (monthly).
- Embed is sold **in every currency Paddle supports**. Buyers see prices in their local currency. The Bubble site shows the local price too (to be built later).

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

**Monthly:** itemised by band, so checkout and invoices show how the total is made up. Each line is quantity 1 with a non-catalogue price (billing cycle monthly, tax mode `external`, product Embed):

| Line name shown to the customer (300 people, GBP) | Amount |
|---|---|
| Places 1 to 25: free | £0.00 |
| Places 26 to 250: 225 at £5.00 a month | £1,125.00 |
| Places 251 to 300: 50 at £4.375 a month | £218.75 |
| **Subtotal** | **£1,343.75** |

- Each line amount = places in band × monthly band price, rounded to the smallest currency unit.
- Show only the bands that have places in them, and always show the free line.
- In GBP the total is identical to rounding the whole total, because only the 251–500 line can have a half penny. In other currencies, rounding each line can differ from the whole-total rounding by one smallest unit at most.
- Checked in a sandbox preview (27 November 2026): £1,343.75 + £268.75 UK VAT = £1,612.50.
- When the number of people changes, the app replaces **only the lines whose amount changed**. An unchanged line keeps its price ID, so the pro-rata bill and email show only what actually changed.
- **Put the breakdown in the product name, not the price name.** Paddle's receipt emails show the product name and the billing cycle ("Monthly"), not the price name. Each line therefore uses a non-catalogue product named, for example, "Embed: places 26 to 250 (225 × £5.00)", with the price name "Monthly". Emails show amounts including tax.

### Sandbox test results (27 November 2026)

- 300 people, monthly: checkout paid with Paddle's test card on the Bubble `paddle-test` branch page (`/pay`). Paddle charged £1,343.75 + £268.75 VAT = £1,612.50 and created an active monthly subscription with the three lines and `org_id` in its custom data. The next renewal is 27 December 2026.
- Lesson: the first receipt email showed "Embed / Monthly" on every line, so the breakdown wasn't visible. Fixed by using product names as described above.
- Increase from 300 to 320 people on day 1: Paddle charged £87.49 + £17.50 VAT = £104.99 pro rata straight away. The next renewal is £1,431.25 + £286.25 VAT = £1,717.50.

**When `n` = 25 or fewer:** no Paddle subscription. The organisation is created in Paddle only when it first needs to pay.

## Currencies: local pricing strategy

Asked for by David Law, 27 November 2026: rounded local prices in every currency, on the same principle as the UK.

### The rule (same in every currency)

1. **Headline:** a round local "per person a month, paid annually" figure, the equivalent of the UK's £4. It's set from Paddle's exchange rate for £4 (27 November 2026), rounded to a natural price point (5, 7.50, 10, 35, 500, 800, and so on). If that would be more than 8% away from the exchange rate, it's rounded to two significant figures instead (for example 1,800 or 17).
2. **Annual band prices** keep the UK ratios exactly:
   - places 26–250: headline × 12 (UK £48)
   - places 251–500: 87.5% of band 1 (UK £42)
   - places 501–1,000: 75% of band 1 (UK £36)
3. **The same rules everywhere:**
   - the first 25 places are free
   - each rate applies only to the places in its band
   - monthly payment is 25% above the annual monthly equivalent, so paying annually saves 20%
   - monthly totals are rounded to the currency's smallest unit
   - over 1,000 is a custom price.
4. **Review** the table every six months, or sooner if an exchange rate moves more than 15% from the rate used here. Existing subscriptions keep their price until renewal.

### Price table (annual, per place per year, excluding tax)

The last column shows how far band 1 is from a straight currency conversion of £48.

| Currency | Headline a month | 26–250 | 251–500 | 501–1,000 | vs exchange rate |
|---|---|---|---|---|---|
| GBP (base) | 4.00 | 48.00 | 42.00 | 36.00 | — |
| USD | 5.00 | 60.00 | 52.50 | 45.00 | −4.8% |
| EUR | 5.00 | 60.00 | 52.50 | 45.00 | +4.4% |
| AUD | 8.00 | 96.00 | 84.00 | 72.00 | +1.6% |
| CAD | 7.50 | 90.00 | 78.75 | 67.50 | +0.2% |
| NZD | 10.00 | 120.00 | 105.00 | 90.00 | +4.4% |
| CHF | 4.50 | 54.00 | 47.25 | 40.50 | +2.7% |
| SEK | 50.00 | 600.00 | 525.00 | 450.00 | −6.8% |
| NOK | 50.00 | 600.00 | 525.00 | 450.00 | −2.2% |
| DKK | 35.00 | 420.00 | 367.50 | 315.00 | −2.2% |
| PLN | 20.00 | 240.00 | 210.00 | 180.00 | −6.6% |
| CZK | 120.00 | 1,440.00 | 1,260.00 | 1,080.00 | +2.5% |
| HUF | 1,800.00 | 21,600.00 | 18,900.00 | 16,200.00 | +1.5% |
| UAH | 250.00 | 3,000.00 | 2,625.00 | 2,250.00 | +5.6% |
| ILS | 17.00 | 204.00 | 178.50 | 153.00 | −2.3% |
| ZAR | 90.00 | 1,080.00 | 945.00 | 810.00 | −1.1% |
| INR | 500.00 | 6,000.00 | 5,250.00 | 4,500.00 | +2.5% |
| SGD | 7.00 | 84.00 | 73.50 | 63.00 | +3.3% |
| HKD | 40.00 | 480.00 | 420.00 | 360.00 | −2.7% |
| CNY | 35.00 | 420.00 | 367.50 | 315.00 | +0.1% |
| TWD | 170.00 | 2,040.00 | 1,785.00 | 1,530.00 | −1.9% |
| THB | 180.00 | 2,160.00 | 1,890.00 | 1,620.00 | +1.0% |
| JPY | 800 | 9,600 | 8,400 | 7,200 | −4.5% |
| KRW | 7,000 | 84,000 | 73,500 | 63,000 | −0.2% |
| VND | 125,000 | 1,500,000 | 1,312,500 | 1,125,000 | −7.9% |
| BRL | 30.00 | 360.00 | 315.00 | 270.00 | +2.0% |
| MXN | 100.00 | 1,200.00 | 1,050.00 | 900.00 | −2.5% |
| CLP | 5,400 | 64,800 | 56,700 | 48,600 | −0.8% |
| COP | 20,000.00 | 240,000.00 | 210,000.00 | 180,000.00 | +7.1% |
| PEN | 18.00 | 216.00 | 189.00 | 162.00 | −1.1% |

**Left on automatic conversion:**
- ARS and TRY, because high inflation would make fixed prices out of date within weeks. [HUMAN: confirm.]
- RUB, because Paddle doesn't sell to any country in RUB.

### How it's set up in Paddle (sandbox, 27 November 2026)

- Each annual band price carries **country price overrides** for the 29 currencies above. Paddle assigns each country to its currency; for example, 127 countries use USD and 47 use EUR. Automatic conversion stays switched on as the fallback for ARS and TRY.
- Checked by preview:
  - US: $60.00 / $52.50 / $45.00
  - Germany: €60.00 / €52.50 / €45.00
  - Japan: ¥9,600 / ¥8,400 / ¥7,200
  - Australia: A$96 / A$84 / A$72
  - India: ₹6,000 / ₹5,250 / ₹4,500
  - UK: £48 / £42 / £36
- **Monthly (non-catalogue price):** the app creates the price in the organisation's own currency, using:
  - total = `round(band1 × B1 × 1.25 / 12 + band2 × B2 × 1.25 / 12 + band3 × B3 × 1.25 / 12)`, to the currency's smallest unit
  - B1, B2 and B3 are that currency's annual band prices from the table.

  Example: 300 people in USD = 225 × $6.25 + 50 × $5.46875 = $1,679.69 a month.
- **A subscription's currency is fixed** when it's created. If an organisation moves country, it keeps its currency until someone changes it by hand.
- **Showing prices on the Bubble site:** ask Paddle for a pricing preview with the visitor's country (Paddle.js `PricePreview`, or `pricing-preview` from the back end). It returns these local prices.

[HUMAN: the brief only states GBP prices. Add the local headline figures to `docs/embed-brief.md` before any marketing copy quotes a non-GBP price.]

[HUMAN: the Paddle balance and payouts are in USD in the sandbox. Confirm which currency Learnt.ai wants payouts in on the live account (GBP, for example).]

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
2. **Daily sync (once a day, per paying organisation):** recalculate `n` and compare it with what's billed.
   - **Up:** update the subscription items with `proration_billing_mode: prorated_immediately`. Paddle charges the pro-rata difference straight away.
   - **Down:** update the items with `proration_billing_mode: do_not_bill`. No credit is given, and the next renewal bills at the lower number.
   - **Down to 25 or fewer:** cancel with `effective_from: next_billing_period`, so the organisation keeps what it paid for until renewal and then has no subscription. If it climbs back above 25 before then, remove the scheduled cancellation.
   - **No change:** do nothing.
   - Always preview the update first (`subscriptions.preview`) and log the amount against the organisation.
3. **Webhooks:** listen for `subscription.created`, `subscription.updated`, `subscription.canceled`, `transaction.completed` and `transaction.payment_failed`, and store the subscription status on the organisation.
4. **Billing self-service:** use Paddle's customer portal (one portal session per request) for invoices, payment method and cancellation.

## Webhook (built and tested on Bubble branch `paddle-test`, 27 November 2026)

**Where:** Bubble app `learntai`, branch `paddle-test` (version `13psj`), backend workflow `paddle_webhook`. Sandbox URL: `https://learnt.ai/version-13psj/api/1.1/wf/paddle_webhook`. Paddle sandbox notification destination `ntfset_01m8egspqwjah2dxxcmvrxmzbj`.

**Events sent:** `subscription.created`, `.updated`, `.activated`, `.canceled`, `.past_due`, `.paused`, `.resumed`, `transaction.completed` and `transaction.payment_failed`. Only `subscription.*` events change data. The others are logged only.

**Security: verify by fetching back.** Bubble can't check Paddle's signature. So the workflow uses only the `notification_id` it receives, then fetches that notification from Paddle's API using a private sandbox API key, held in the Bubble API Connector collection "Paddle Billing (sandbox)". Only data returned by Paddle is trusted. Tested: a forged "cancelled" call with a made-up notification ID failed at the Paddle lookup and changed nothing.

**Steps:**
1. Log the event (`Embed_PaddleEvent`: event ID, type, notification ID, processed = no).
2. Stop if it isn't a `subscription.*` event.
3. Stop if this event ID has already been processed. Paddle retries, so the same event can arrive twice.
4. Fetch the notification from Paddle (`get_notification`).
5. Stop if no Organisation matches `custom_data.org_id` (the Organisation's unique ID).
6. Update the Organisation, but only if this event is newer than `billing_synced_at`, because events can arrive out of order. Fields updated:
   - `paddle_customer_id`, `paddle_subscription_id`
   - `billing_status` (active / past_due / paused / canceled)
   - `billing_cycle` (month / year), `billing_currency`
   - `billed_places` (from `custom_data.billable_people`)
   - `billing_next_billed_at`, `billing_period_ends_at`, `billing_scheduled_change`
   - `billing_synced_at`.
7. Mark the log row processed and link it to the Organisation.

**Privacy:** `Embed_PaddleEvent` is visible only to Embed owners. Organisation admins can see their own organisation's billing fields through the existing "Admin — own organisation" rule. Members can't see them.

**Tested end to end (27 November 2026):** changing the sandbox subscription's `org_id` to a test Organisation produced a `subscription.updated` event. The workflow filled in all the billing fields: active, month, GBP, 320, next bill 27 December 2026.

**Known gaps:**
- `billing_scheduled_change` was typed as text when the call was set up, because the sample had no scheduled change. Re-initialise `get_notification` with an event that has one (for example a cancellation scheduled for the end of the period) so Bubble learns its fields.
- The checkout flow must put the Organisation's unique ID in `custom_data.org_id` and the billable count in `custom_data.billable_people` on every subscription it creates or updates.
- [HUMAN: before going live, create a live API key with minimum permissions (Notifications read, Subscriptions read and write, Customers read and write, Transactions read and write), a live collection or a switch in Bubble, and a live notification destination pointing at the live app URL.]
- [HUMAN: delete the test Organisation "Paddle Sandbox Test Organisation (delete after testing)" from the development database once testing is finished.]

## Self-serve checkout (built on Bubble branch `paddle-test`, 27 November 2026; not yet tested by clicking)

**Decisions (David Law, 27 November 2026):**
- **Billable person:** someone with at least one active content set assignment on a **live** content set (content set `active` = yes and `archived_at` empty). Each person counts once.
- **Above 25:** the 26th person is blocked until the organisation has paid. [Not built yet: needs a check added to the assignment workflows.]
- **Currency:** Paddle picks it from the buyer's location. The admin doesn't choose.

**Page:** `embed_admin_billing`, a copy of the admin settings page, so it keeps the side menu, branding and the redirects for people who aren't logged in or aren't admins. It shows the number of people on live content sets, then one of:
- the current plan, if subscribed
- "You're using N of your 25 free places", under 25
- the upgrade message and **Pay monthly** / **Pay annually** buttons, from 25 to 1,000
- "Talk to us about a custom price", over 1,000.

**What the buttons do:**
1. A Toolbox **Server script** (on Bubble's server) builds the Paddle transaction. Places start at max(count, 26), split into bands, with lines named by band, local prices for 29 currencies as country overrides, and `custom_data` holding `org_id`, `billable_people` and `cycle`. No checkout URL is sent. Paddle's default payment link (sandbox: `https://learnt.ai/version-13psj/pay`) is only a fallback.
2. API Connector `create_transaction` (POST `/transactions`) sends it to Paddle.
3. Paddle's **overlay checkout** opens on top of the billing page: Paddle.js is in the page header, and a Toolbox "Run javascript" step calls `Paddle.Checkout.open({transactionId})`. When payment completes, the page reloads after 4 seconds to show the plan. After payment, Paddle creates the subscription, which carries the same `custom_data`, and the webhook fills in the Organisation's billing fields.

**Checked:** the script's GBP totals match the brief for 26, 100, 300 and 1,000 people, annual and monthly. Paddle previews for the UK, US and Germany use GBP, USD and EUR as expected.

**Lesson (Toolbox Server script):** dynamic values inserted into the script text make it fail, and so does "Website home URL" as an input. Keep the script text fixed and pass live values through its key/value inputs, which the script reads as `properties.keyvaluesobj`.

**Not built yet:** the block at 25, a "Billing" link in the admin side menu, showing an error message if checkout can't be created, and the daily update.

## Approving increases: user experience (decided by David Law, 27 November 2026)

**Principle:** the admin consents once at checkout and sees the cost at the moment it changes. There's no approval step for each increase.

1. **Consent at checkout.** The billing page states, above the pay buttons: "You pay for each person on a live content set beyond the first 25. When you add people, we charge the difference straight away for the rest of your billing period. When you remove people, your bill goes down at your next renewal."
2. **Increases are charged straight away, on both monthly and annual plans** (pro rata), by the daily update.
3. **Place limit (optional, off by default).** On the billing page an admin can set a maximum number of billable places. When it's set and an assignment would exceed it, the new people are not activated, and admins are emailed to raise the limit. When it's empty, there's no limit.
4. **Organisations not yet paying** are blocked at 25 billable people until checkout is complete. Once paying, only the place limit (if set) blocks.
5. **Cost preview on the content set page.** When an admin assigns a content set or makes it live, show "This adds N billable people: about £X now, then £Y a month (or year)" before they confirm. People already counted aren't counted again. Not on the user management page, because the number of users makes no difference to the price: only people on live content sets count.
6. **Emails.** Paddle sends a receipt for every charge. The app also emails admins when the number of billed places changes. [Not built yet.]
7. **Daily update:** one update a day, so small additions don't become many tiny charges.

**Build order:** checkout wording → place limit and block at 25 → daily update → cost preview on the content set page → emails.


### Built so far (paddle-test branch, 27 November 2026)

- **Consent wording** is on the billing page above the pay buttons.
- **Place limit:** new Organisation field `billing_place_limit`. Paying admins see a "Place limit (optional)" box on the billing page. Empty means no limit.
- **Enforcement:** a check at the start of three backend workflows:
  - `wf_assign_content_set_to_user` (manual) and `9A wf_upsert_live_content_set_assignment` (automatic audiences) run only if the content set isn't live, **or** the person is already billable, **or** the organisation is paying and under any limit, **or** the organisation has fewer than 25 billable people.
  - `1. wf_go_live_content_set` runs only if everyone the set would make billable still fits: within 25 if not paying, within the limit if paying.
- People held back from automatic audiences are retried by the daily audience sync, so they're added once the limit is raised.
- **Assignment refusal (content set page):** clicking Assign first works out how many billable people there would be afterwards (page state `limit_after_count`). If it's allowed, the assignments and the "updated" message run as before. If not, nothing is assigned, there's no alert, and a pop-up ("Nobody has been assigned") explains why and has Close and Go to Billing buttons. The pop-up says one of two things: the place limit has been reached (with the limit and the new count), or the organisation isn't paying and has gone over the free 25. The backend gate still refuses anything that gets past the page.
- **Design pass (matches the admin pages):** the Billing page now uses the admin card style: a rounded card with a border and a soft shadow, a 14px bold card title and 12px body text in the primary colour. It has three cards:
  1. **An overview (an HTML element)** showing the number of places in use, a meter (the first 25 free shown striped, the place limit marked in orange), plan tiles (places billed, billing cycle, next payment, currency) and a three-step "How billing works" section. Colours come from the app's colour tokens, motion is turned off when the browser asks for reduced motion, and below 820px it drops to two columns.
  2. **Add more people:** shown only when the organisation isn't paying.
  3. **Place limit:** shown only when the organisation is paying.

  The refusal pop-up follows the go-live pop-up's pattern: a close icon, an icon and heading, 40px margins, an info box showing the limit (or the free 25) and the count after assignment, and Close / Go to Billing buttons built as groups styled like the admin buttons.
- **Limit can't go below current use:** saving a place limit lower than the number of people on live content sets is refused. The organisation isn't changed, there's no "saved" message, and a pop-up ("The limit can't go that low") shows the limit entered and the current count. It tells the admin to unassign people first, with Close and Go to Content sets buttons. An empty limit (no limit) can always be saved.
- **Paid places tile:** shows billed places minus the free 25 (never below 0), with "N in total, first 25 free" underneath. Paddle already charges only places 26 and up, so this is display only.
- **Known gap:** a refused go-live still shows no message. The cost preview and admin emails aren't built yet.
- Savepoints: `1795793642810` (before wording and limit).

## Still to do

- [HUMAN: webhook endpoint URL in the Bubble app, then create the notification setting in sandbox and test with Paddle simulations]
- [HUMAN: client-side token for Paddle.js checkout in Bubble (sandbox token starts `test_`)]
- [HUMAN: approve copying the catalogue to live; a person switches it on]
- Bubble set-up and end-to-end testing: parked until Paddle is set up (David Law, 27 November 2026).

ready for human: no (draft)
