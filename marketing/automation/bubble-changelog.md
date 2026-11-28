# Bubble change log: app `learntai`

Every major change to the Bubble app starts with a savepoint. This log records what each one protects, so you can see what restoring it would undo.

## How to read and use this log

- **Newest first.** Each entry is one savepoint plus the changes made after it.
- **Restoring a savepoint undoes every change listed in that entry and every entry above it** on the same branch.
- **To restore:** open the Bubble editor, go to Settings, then Versions (Version control). Choose the branch, find the savepoint by its name or time, and restore it. Restoring is a human action.
- **Times** are UTC. The savepoint number is Bubble's timestamp, which the restore tool also accepts.

## Savepoint naming

From 28 November 2026, savepoints are named like this:

`SP-NNN · Before: <what is about to change> · <branch>`

For example: `SP-008 · Before: daily Paddle sync workflow · paddle-test`

Savepoints SP-001 to SP-007 were made before this naming rule existed, so Bubble shows them by their original messages, listed below.

---

## SP-010 · Before: in-app billing (invoices, card, cancel)

- **Bubble name:** "SP-010 · Before: in-app billing (invoices list, update card, cancel and keep plan) · paddle-test"
- **Branch:** paddle-test (`13psj`) · **Time:** 28 Nov 2026 10:25 UTC · **Timestamp:** 1795861514592
- **Changed after it:**
  - **API Connector, three new calls:**
    - `list_transactions` (completed, billed and past-due payments for the subscription)
    - `get_invoice_pdf` (a short-lived download link for one invoice)
    - `get_update_card_transaction` (the transaction Paddle needs to change the card).
  - **Organisation fields:**
    - `billing_cancel_requested`: the admin cancelled.
    - `billing_auto_cancelled`: the daily sync cancelled because the organisation dropped to 25 or fewer.
  - **Billing page, "Payments and invoices" card:**
    - new wording explaining that Paddle is the payment provider and reseller
    - an "Update payment card" button, which opens Paddle's card form over the page
    - a "Cancel plan" button, which opens a confirmation pop-up
    - "Keep my plan" and a notice showing the end date, while a cancellation is pending
    - an invoice list loaded when the page opens (date, invoice number, amount, Download), with a message when there are no payments yet
    - the portal button relabelled "More billing options" and restyled as a secondary button.
  - **Cancel pop-up (Popup B)**, built in the admin style. "Cancel plan" schedules the cancellation at the end of the period. "Keep my plan" (in the pop-up, and on the page while a cancellation is pending) removes it.
  - **Daily sync:**
    - It passes `billing_auto_cancelled` to the script, which leaves any cancellation it didn't make itself alone (`SKIP:cancel_requested`).
    - It sets `billing_auto_cancelled` to yes when it cancels, and back to no when it removes its own cancellation.
- **Restoring it undoes:** the in-app invoices, card update and cancellation, and the sync's cancellation safeguard. Also everything in later entries.

## SP-009 · Before: Manage billing and the upgrade pop-up

- **Bubble name:** "SP-009 · Before: Manage billing (Paddle customer portal) and upgrade pop-up at 26 people · paddle-test"
- **Branch:** paddle-test (`13psj`) · **Time:** 28 Nov 2026 09:53 UTC · **Timestamp:** 1795859599060
- **Changed after it:**
  - **API Connector:** new call `create_portal_session`: POST /customers/{id}/portal-sessions with the subscription ID. It returns a short-lived link to Paddle's customer portal.
  - **Billing page:** a new card, "Payments and invoices", shown once the organisation has a Paddle customer ID. Its "Manage billing and invoices" button creates a portal session and opens Paddle's portal, where the admin can update their card, download invoices and receipts, see payment history or cancel.
  - **Content set page, refusal pop-up:** when the organisation isn't paying and an assignment would take it past 25, the pop-up becomes an upgrade prompt:
    - title "Add more people with a paid plan" and a rocket icon
    - a message explaining that the first 25 are free, that you pay only for people beyond 25, and that a place limit can keep spend under control
    - the box shows free places, the count after this assignment, and the paid places
    - buttons "Not now" and "Choose a plan" (which goes to Billing).

    The wording when the place limit is reached is unchanged.
- **Restoring it undoes:** the portal button, the API call, and the upgrade wording, so the plain refusal message comes back. Also everything in later entries.

## SP-008 · Before: daily Paddle sync

- **Bubble name:** "SP-008 · Before: daily Paddle sync (API calls, sync workflow, recurring event, Organisation fields) · paddle-test"
- **Branch:** paddle-test (`13psj`) · **Time:** 28 Nov 2026 09:07 UTC · **Timestamp:** 1795856870583
- **Changed after it:**
  - **API Connector** (collection "Paddle Billing (sandbox)"), three new calls, all returning raw text:
    - `get_subscription_raw`: GET /subscriptions/{id}.
    - `update_subscription`: PATCH /subscriptions/{id}; the body is a parameter.
    - `cancel_subscription_at_period_end`: POST /subscriptions/{id}/cancel with `effective_from: next_billing_period`.
  - **Organisation fields:** `billing_sync_scheduled` (yes/no), `billing_sync_result` (text), `billing_sync_checked_at` (date).
  - **Backend workflow `billing_sync_org`** (parameter: org). Steps:
    1. Fetch the subscription from Paddle.
    2. A server script compares the live billable count with what's billed and returns NONE, CANCEL, SKIP or ERROR, or an update body.
    3. If it returned an update body, send it to Paddle.
    4. If it returned CANCEL, schedule the cancellation at the end of the period.
    5. Record the result and the time on the Organisation.
  - **Recurring event `billing_daily_sync`** (on Organisation): runs `billing_sync_org` every day.
  - **`paddle_webhook`:** two new steps after "Make changes to Organisation". The first time an active subscription event arrives for an organisation, they start the daily schedule at 03:00 UTC the next day and set `billing_sync_scheduled` to yes.
  - **Billing page:** an owner-only link, "sync this organisation with Paddle now", that runs `billing_sync_org` immediately (for testing and support).
- **Outside Bubble (Paddle sandbox):** three band products were created for the sync's line items:
  - `pro_01m8gpgqy6z188ck3ppvjj4ajy` (places 26 to 250)
  - `pro_01m8gpgr11k18b98asy8pd2ey3` (places 251 to 500)
  - `pro_01m8gpgr46g9nqwaprx0avcyrq` (places 501 to 1,000)

  Restoring the savepoint doesn't remove them.
- **Restoring it undoes:** the daily sync, its schedule, the fields and the three API calls. Also everything in later entries.

## SP-007 · Before: restyling pop-up buttons and info boxes

- **Bubble name:** "Before restyling pop-up buttons and info boxes on content set and billing pages"
- **Branch:** paddle-test (`13psj`) · **Time:** 27 Nov 2026 17:18 UTC · **Timestamp:** 1795799882383
- **Changed after it:**
  - Refusal pop-up (content set page) and place-limit pop-up (Billing page): buttons restyled to match "Assign selected". The primary button is filled in the primary colour; Close has a white fill with a primary-colour border. Both have hover states and centred labels.
  - Both info boxes changed from a teal tint with an icon to a white box with a grey border and no icon.
- **Restoring it undoes:** the button and box restyle only.

## SP-006 · Before: place-limit-too-low pop-up and paid places tile

- **Bubble name:** "Before adding the place-limit-too-low pop-up and paid places tile on embed_admin_billing"
- **Branch:** paddle-test · **Time:** 27 Nov 2026 17:03 UTC · **Timestamp:** 1795798986690
- **Changed after it:**
  - Billing page: saving a place limit lower than the number of people on live content sets is refused. A pop-up ("The limit can't go that low") shows the limit entered and the current count, with Close and Go to Content sets buttons.
  - Billing overview: the "Places billed" tile became "Paid places", showing billed places minus 25 (never below 0) with "N in total, first 25 free" underneath.
- **Restoring it undoes:** the limit check, its pop-up, and the paid places tile. Also everything in SP-007.

## SP-005 · Before: restyling the Billing page and refusal pop-up

- **Bubble name:** "Before restyling the billing page and the assignment refusal pop-up to match the admin site"
- **Branch:** paddle-test · **Time:** 27 Nov 2026 16:47 UTC · **Timestamp:** 1795798023050
- **Changed after it:**
  - Billing page rebuilt as admin-style cards:
    - an HTML overview with the places in use, a meter, plan tiles and "How billing works"
    - an "Add more people" card, shown when the organisation isn't paying
    - a "Place limit" card, shown when it is paying.
  - Refusal pop-up on the content set page restyled to match the go-live pop-up: close icon, icon and heading, an info box, and Close and Go to Billing buttons.
- **Restoring it undoes:** the Billing page redesign and the pop-up restyle. Also everything in SP-006 and SP-007.

## SP-004 · Before: limit-refusal pop-up on the content set page

- **Bubble name:** "Before limit-refusal popup on embed_admin_content_set"
- **Branch:** paddle-test · **Time:** 27 Nov 2026 16:32 UTC · **Timestamp:** 1795797154882
- **Changed after it:**
  - Content set page, Assign button:
    - It now first works out how many people would be billable afterwards (page state `limit_after_count`).
    - If that's allowed, it assigns and shows "Assignments updated" as before.
    - If it's blocked, nothing is assigned, there's no alert, and the "Nobody has been assigned" pop-up appears.
- **Restoring it undoes:** the clear refusal on the page; refused assignments would show the old "updated" alert again. The backend gate stays. Also everything in SP-005 to SP-007.

## SP-003 · Before: billing consent wording and place limit

- **Bubble name:** "Before billing consent wording and place limit"
- **Branch:** paddle-test · **Time:** 27 Nov 2026 15:34 UTC · **Timestamp:** 1795793642810
- **Changed after it:**
  - The consent wording at checkout.
  - The organisation field `billing_place_limit`, plus the place limit input and Save button on the Billing page.
  - The billing gates on three workflows (`wf_assign_content_set_to_user`, `9A wf_upsert_live_content_set_assignment` and `wf_go_live_content_set`) now also block anything over the place limit. Some of the gates may have been added before this savepoint; check the conditions after restoring.
- **Restoring it undoes:** the place limit and the gates' place-limit checks. Also everything in SP-004 to SP-007.

## SP-002 · Before: switching to the Paddle overlay checkout

- **Bubble name:** "Before switching embed_admin_billing to Paddle overlay checkout"
- **Branch:** paddle-test · **Time:** 27 Nov 2026 15:13 UTC · **Timestamp:** 1795792401289
- **Changed after it:** the Pay monthly and Pay annually buttons open Paddle's overlay checkout on the page (Paddle.js in the page header) instead of going to a separate checkout page. The page reloads after payment.
- **Restoring it undoes:** the overlay checkout. Also everything in SP-003 to SP-007.

## SP-001 · Before: checkout workflows on the Billing page

- **Bubble name:** "Before checkout workflows on embed_admin_billing (Paddle self-serve checkout)"
- **Branch:** paddle-test · **Time:** 27 Nov 2026 14:46 UTC · **Timestamp:** 1795790799137
- **Changed after it:** the self-serve checkout on the Billing page:
  - a billable-count server script
  - the API call `create_transaction`
  - the Pay monthly and Pay annually workflows.
- **Restoring it undoes:** self-serve checkout and everything since.

## Before SP-001 (no savepoint)

Built on the `paddle-test` branch before the savepoint rule. These are recorded in `paddle-billing-spec.md`:

- the `pay` test page and the `embed_admin_billing` page
- the billing fields on Organisation
- the `Embed_PaddleEvent` data type
- the `paddle_webhook` backend workflow
- the Paddle Billing (sandbox) API collection with `get_notification`.
