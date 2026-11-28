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
