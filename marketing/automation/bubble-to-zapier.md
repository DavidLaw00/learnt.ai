Purpose: How the Embed app (Bubble) sends events to Zapier, which updates HubSpot and alerts Microsoft Teams.
Audience: Internal (Dave, building in Bubble and Zapier)
Channel / placement: Bubble → Zapier → HubSpot and Microsoft Teams
Brief sections used: Getting started (four steps), Pricing (25 free), Security and data (controller and processor roles)
Research used: none yet. Plan requirements are [VERIFY]
Experiment: none (tracking backbone)
Launch timing: post-launch (sign-ups), and pre-launch testing
Status: ready for human

# Bubble to Zapier: sending Embed events

**Yes, Bubble can do this.** The standard way is for Bubble to call a **Zapier webhook**:

1. Zapier gives you a private web address (a "catch hook").
2. Bubble's **API Connector** plugin sends a small package of data to that address whenever something happens in the app.
3. Zapier then updates HubSpot and posts in Teams.

## Rule one: send the account owner, never their people

When someone signs up and starts an Embed account, they're **your** prospect or customer, and HubSpot can hold their details.

The people they add to Embed (CSV uploads or invites) are **their** staff. The brief says Learnt.ai is only the Data Processor for that data, so it must never go to HubSpot, Teams or Zapier. For those people, send **counts only**.

This affects how you trigger events: a "new User created" trigger would fire for invited people too. See step 3.

## Plan checks before you start

- **Zapier:** "Webhooks by Zapier" and multi-step zaps with Paths may need a paid Zapier plan `[VERIFY on your plan]`. If Paths aren't available, use one catch hook and one zap per event instead.
- **Bubble:** backend workflows and database trigger events may need a paid Bubble plan `[VERIFY on your plan]`. If they're not available, use the front-end option in step 3.

## Events to send

| Event | When it fires | Data to send | What Zapier does |
|---|---|---|---|
| `account_created` | Someone signs up and becomes the owner or admin of a new Embed account | `event`, `account_id`, owner `email`, `first_name`, `last_name`, `job_title` (if asked), `organisation_name`, `organisation_domain`, `utm_source`, `utm_medium`, `utm_campaign`, `utm_content`, `timestamp` | Create or update HubSpot contact and company; set sign-up date, first-touch fields (only if empty) and lawful basis "Contract"; create or move the deal to **Signed up**; alert Teams |
| `users_milestone` | The account's user count first reaches 5, 20, 25 or 26 | `event`, `account_id`, owner `email`, `users_count`, `milestone`, `timestamp` | Update company users count. At 20 and 25, alert Teams: "nearing or at the free 25: founder to offer a call". At 26, note that the paid band has started |
| `content_generated` | The account's first content set is generated | `event`, `account_id`, owner `email`, `timestamp` | Add a note to the contact (activation step 2 of 4) |
| `content_assigned` | The first content set is assigned to a group | `event`, `account_id`, owner `email`, `timestamp` | Add a note (activation step 3 of 4) |
| `went_live` | The first person in the account answers their first practice question | `event`, `account_id`, owner `email`, `timestamp` | Set the went-live date; move the deal to **Live**; alert Teams |
| `plan_changed` | The account moves between free and paid, or changes billing | `event`, `account_id`, owner `email`, `plan` (free, paid_annual, paid_monthly, custom), `paid_places`, `timestamp` | Update the company's plan and paid places; a paid plan moves the deal to **Paid (won)**; alert Teams |

The events follow the brief's four getting-started steps (add users, generate content, assign, go live). You'll see exactly where new accounts stall.

Names of things in your Bubble database (account, organisation, content set, practice answer) are guesses `[HUMAN: map to your actual data types]`.

---

## Step by step

### 1. Zapier: create the catch hook

1. Create a Zap. For the trigger, choose **Webhooks by Zapier → Catch Hook**.
2. Copy the hook URL. **Treat it like a password:** don't put it in this repository, a public page or an email.

### 2. Bubble: add the call

1. Plugins → install **API Connector** (by Bubble).
2. Add an API called `Zapier`. Add a call called `Send event`:
   - **Use as:** Action
   - **Method:** POST, to the hook URL
   - **Body type:** JSON, with each field from the table as a dynamic parameter. For example:
     ```json
     {
       "event": "<event>",
       "account_id": "<account_id>",
       "email": "<email>",
       "first_name": "<first_name>",
       "last_name": "<last_name>",
       "organisation_name": "<organisation_name>",
       "organisation_domain": "<organisation_domain>",
       "users_count": "<users_count>",
       "plan": "<plan>",
       "utm_source": "<utm_source>",
       "utm_medium": "<utm_medium>",
       "utm_campaign": "<utm_campaign>",
       "utm_content": "<utm_content>",
       "timestamp": "<timestamp>"
     }
     ```
   - Mark the URL as **private**, so it never reaches the browser.
3. **Initialise** the call once with test values, so Bubble and Zapier both learn the fields.

### 3. Bubble: fire the events

**Recommended: backend.** Use **Backend workflows → Database trigger events**:
- **`account_created`:** "An Account (organisation) is created", not "a User is created". This stops invited staff from triggering it.
- **`users_milestone`:** "An Account's user count changes", where the count before was below a milestone and is now at or above it.
- **`went_live`, `content_generated`, `content_assigned`, `plan_changed`:** the first time each happens for the account. Keep yes/no flags on the account, such as `went_live_sent`, so each event fires only once.

Each trigger runs the action **Zapier – Send event**.

**Front-end fallback** (if backend workflows aren't on your plan): add **Zapier – Send event** as the last step of the sign-up workflow, after "Sign the user up" and creating the account. This is less reliable. It can miss sign-ups through other routes, such as social login, and only fires if the page finishes.

### 4. Bubble: keep the UTM tags

Without this step, you can't tell which experiment produced a sign-up.

1. On page load of the landing and sign-up pages, read **Get data from page URL** for `utm_source`, `utm_medium`, `utm_campaign` and `utm_content`. Store them in custom states, or a cookie if visitors move between pages.
2. When the account is created, save them on the account, **only if empty**, so the first touch is kept.
3. Is the learnt.ai website the same Bubble app as the sign-up? `[HUMAN: confirm]` If not, the website's "Get started free" links must pass the UTM tags through to https://learnt.ai/signup.

### 5. Zapier: route each event

After the catch hook, add **Paths**, split on `event`, or one zap per event. For each path:

1. **Filter:** skip the event if the HubSpot contact has `embed_do_not_contact` set. Alerts are fine; skip any marketing step.
2. **HubSpot → Create or update contact** (matched on email), then **Find or create company** (on domain), then associate them.
3. **HubSpot → Create or update deal** at the stage in the table above.
4. **Microsoft Teams → Send channel message**. Use a short alert such as:
   > New Embed sign-up: {organisation_name} ({utm_campaign}). Owner: {first_name}, {job_title}. Open in HubSpot: {link}

   Teams alerts go to an internal channel only.

### 6. Test

1. Sign up a test account using a link with test UTM tags, for example `?utm_source=test&utm_campaign=EXP-R0-00`.
2. Check Zapier's history, the HubSpot contact, company and deal, and the Teams message.
3. Add 25 test users and check the milestones fire once each, **and that no test user's name or email reaches Zapier**.
4. Delete the test records.

## Things to confirm

- `[HUMAN]` Your Bubble and Zapier plans (backend workflows, webhooks, Paths).
- `[HUMAN]` The real names of the account, owner and content data types in your Bubble app.
- `[HUMAN]` Whether the learnt.ai website is the same Bubble app as the sign-up.
- `[HUMAN]` Whether sign-up asks for job title. It helps qualify leads, but adds a field to the 20-minute set-up.
