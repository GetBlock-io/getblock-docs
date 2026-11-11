---
description: >-
  What GetBlock webhooks cost in CU, the statuses a webhook can be in, what
  happens when the CU balance runs out, and the webhook limits of each plan.
---

# Pricing and Limits

### What is billed

**One delivery attempt = 25 CU**, taken from the same CU balance as your RPC requests.

An attempt is one HTTP request to your endpoint. Every attempt of a live event is billed, whatever its outcome:

| Delivery | Billed |
| --- | --- |
| The unconfirmed-phase delivery (`"confirmed": false`) | Yes |
| The confirmed-phase delivery (`"confirmed": true`) | Yes |
| A reorg correction (`"removed": true`), including the removal of an event this webhook never received | Yes |
| A retry — each of up to 8 attempts of one event | Yes |
| A failed attempt, the last one included | Yes |
| An attempt that got no HTTP response: the host did not resolve, the connection was refused or timed out, TLS failed, or the host resolved to a private or reserved address | Yes — it counts as a failed attempt |
| A test delivery | **No** |

What this means per event:

* A webhook with **both phases** pays twice per event (2 × 25 CU), and three times when a reorg sends a correction too.
* An endpoint that fails every attempt costs up to **8 × 25 = 200 CU per event** before the event is given up — and then [auto-pause](retries-and-endpoint-protection.md#endpoint-protection) stops the webhook.
* Nothing is billed when no attempt is made: an event waiting for your endpoint's concurrency or rate limit, an event dropped above your plan's events-per-second limit, an event of a paused webhook, or an attempt skipped while the circuit breaker is open. (Skipped attempts still count in `failed_total` of the webhook's stats.)
* CU already spent are not returned, also when a reorg later removes the event.

Owners of dedicated or limitless nodes pay for webhooks from the same CU balance, with the Free plan's webhook limits. A team account needs a plan to create webhooks (`403 plan_required`).

For CU in general, see [Plans and Limits](../getting-started/plans-and-limits/README.md).

### Webhook statuses

| `status` | Dashboard | Set by | Deliveries | How it ends |
| --- | --- | --- | --- | --- |
| `active` | Active | Create, resume | Yes | — |
| `paused` | Paused | You (**Pause**, `POST …/pause`) | No; queued events are dropped | **Resume**, `POST …/resume` |
| `paused_auto` | Paused automatically | GetBlock, after 30 minutes of failures (at least 20 attempts) | No; queued events are dropped as `auto_paused` | Fix the endpoint, then resume |
| `paused_plan` | Paused: over plan limits | GetBlock, when a plan change leaves the webhook over the plan's limits | No | An upgrade resumes it by itself; or resume it once it fits |
| `paused_no_balance` | Paused: out of CU | GetBlock, after 30 minutes at zero balance | No; queued events are dropped as `no_balance` | Top up, then resume |
| `deleted` | — | You (**Delete**, `DELETE …`) | No | Final |

**In every paused status, events are not matched.** They are not queued while the webhook waits and are never sent after it resumes: the loss lasts until the resume.

Pausing a webhook that GetBlock paused turns it into your own pause: an upgrade or a top-up then no longer applies to it.

After a downgrade, the webhooks that still fit the new plan stay active — the newest first — and the rest become `paused_plan`. An upgrade brings them back, newest first.

### When the balance runs out

1. **The balance runs out.** Deliveries of all your webhooks stop within one to two minutes. Events wait without being sent or billed — up to 10 minutes, then through their retries; a top-up before the webhook is paused lets them through.
2. **After 30 minutes at zero**, each active webhook that has events to deliver becomes **`paused_no_balance`**. Its queued events are dropped and recorded as `no_balance`.
3. **You top up and resume the webhook** (**Resume** in the dashboard, or `POST /api/v1/webhooks/{id}/resume`). **Nothing resumes it automatically.** A resume while the balance is still empty answers `409 insufficient_balance` and changes nothing. After a top-up the balance takes one to two minutes to clear; a resume in that gap is accepted, and deliveries start as soon as it clears.

{% hint style="warning" %}
* **Events that occur while a webhook is paused for balance are never delivered** — they are lost until you resume.
* **On the Free plan the daily CU (50,000 CU, about 2,000 delivery attempts) can run out.** The webhook then stays paused until you resume it after the daily top-up at 07:30 UTC; it does not come back by itself the next day.
{% endhint %}

Test deliveries are free and are sent even at zero balance, as long as the webhook is `active`.

### Plan limits

| Plan | Active webhooks | Addresses per webhook | Addresses per account | Events per second | Burst |
| --- | --- | --- | --- | --- | --- |
| Free / Lite | 1 | 10 | 10 | 1 | 12 |
| Starter | 10 | 100,000 | 100,000 | 5 | 60 |
| Growth / Advanced | 25 | 100,000 | 250,000 | 10 | 120 |
| Scale | 50 | 250,000 | 1,000,000 | 20 | 240 |
| Pro | 75 | 250,000 | 1,000,000 | 30 | 360 |
| Premium | 100 | 250,000 | 1,000,000 | 50 | 600 |
| Enterprise | Premium's limits, unless your contract says otherwise | | | | |

* **Active webhooks** counts every webhook that is not deleted, paused ones included: a pause does not free a slot, a delete does.
* **Addresses per webhook** is the sum of the webhook's own addresses and every address list it references, **without de-duplication**: an address in two lists counts twice. One list holds at most 100,000 addresses, so 250,000 needs at least three lists (for example 100,000 + 100,000 + 50,000).
* **Addresses per account** is the sum of the addresses of all your webhooks that are not deleted, paused ones included, counted per webhook the same way and again **without de-duplication**: one list of 100,000 addresses referenced by two webhooks counts 200,000. On Starter a full list can therefore back only one webhook.
* **Events per second** is one budget for all your webhooks. Events above the rate and the burst are **dropped, not delayed**: they are not attempts, are not billed and are not sent later. The webhook's stats count them as `rate_limited_total`.
* **Burst** is how many events may go out at once before the per-second rate applies: 12 × the rate, about one Ethereum block's worth of events.

Other limits, on every plan: 50 named address lists per account, 10 lists per webhook, 1,000 addresses written into the filter itself, and a request body of about 5 MiB — a list of 100,000 addresses (about 4.6 MB) fits in one request.

A request over a limit is refused with `409` and the `cap_*` code of the limit reached (see [Error codes](api-reference/error-codes.md)). Your current limits and usage are on the **Pricing** and **Overview** tabs of the Webhooks page in the dashboard, and in `GET /api/v1/webhooks/limits`.
