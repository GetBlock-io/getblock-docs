---
description: >-
  Create your first GetBlock webhook in the dashboard or through the API,
  save its signing secret, send a test event and verify it on your endpoint.
---

# Getting Started

This guide takes you from an empty account to an endpoint that receives and verifies signed webhook deliveries for your Ethereum wallets.

### 1. Prepare an endpoint

Your endpoint is the URL GetBlock sends events to. It must:

* use `https` on a public domain name, with a valid TLS certificate for that name;
* accept `POST` requests with a JSON body;
* answer with any `2xx` status within 15 seconds;
* read the **raw request body** before parsing it — the signature is computed over the exact bytes (see [Verifying Signatures](verifying-signatures.md)).

{% hint style="info" %}
To look at a first delivery before writing any code, any public request-inspection service with an `https` URL will do. In production, verify the signature of every request.
{% endhint %}

### 2. Create a webhook

{% tabs %}
{% tab title="Dashboard" %}
1. Open [**Products → Webhooks**](https://account.getblock.io/products/webhooks) in your GetBlock account and click **Create webhook**.
2. Pick a template, or set every field yourself. A template fills in the trigger, phases and depth; every field stays editable:
   * **Wallet activity** — any confirmed transaction to or from your addresses;
   * **Token transfers** — ERC-20 `Transfer` events of a token contract, first as soon as seen, then once confirmed;
   * **Transaction confirmations** — a notification once a transaction of your addresses reaches the confirmation depth.
3. Go through the four steps of the form:
   * **Chain & network** — Ethereum, Mainnet.
   * **Trigger** — the trigger type, the phases (**Unconfirmed**, **Confirmed** or both) and the confirmation depth. Leave the depth empty for the chain default.
   * **What to watch** — the addresses, from **Saved lists**, **Manual input** or **Import** (a `.txt` or `.csv` file with one address per line). For a log event, enter the **Contract address** and/or **Topic0**.
   * **Endpoint & review** — the **Endpoint URL** and an optional **Name**, then check the summary and create the webhook.
{% endtab %}

{% tab title="API" %}
1. Log in to your [GetBlock account](https://account.getblock.io/), open [**Settings → API Keys**](https://account.getblock.io/settings/api-keys) and copy a key. Keys start with `gb_`.
2. Create the webhook with `POST /api/v1/webhooks`:

{% code overflow="wrap" %}
```bash
curl -X POST https://public-api.getblock.io/api/v1/webhooks \
  -H "Authorization: Bearer $GETBLOCK_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "treasury-wallets",
    "chain": "eth",
    "network": "mainnet",
    "trigger_type": "address_activity",
    "phases": ["unconfirmed", "confirmed"],
    "target_url": "https://hooks.example.io/getblock",
    "addresses": ["0x742d35cc6634c0532925a3b844bc454e4438f44e"]
  }'
```
{% endcode %}
{% endtab %}
{% endtabs %}

Through the API, the answer is `201 Created` with the webhook and its signing `secret`:

{% code overflow="wrap" %}
```json
{
  "id": "wh_4Qk7Xz9pLm2R",
  "name": "treasury-wallets",
  "status": "active",
  "chain": "eth",
  "network": "mainnet",
  "trigger_type": "address_activity",
  "filters": {
    "type": "address",
    "dir": "any",
    "list_refs": ["al_2bQ4rTz9KmNp"],
    "src": "sugar"
  },
  "confirm_depth": null,
  "phases": ["unconfirmed", "confirmed"],
  "target_url": "https://hooks.example.io/getblock",
  "endpoint_hash": "3f2a9c0e5b7d41a8c6e2f0b9d8a7c6e5f4d3c2b1a0918273645546372819aabb",
  "batching": { "max_events": 0, "max_wait_ms": 0, "gzip": false },
  "version": 1,
  "created_at": "2026-09-25T10:00:00Z",
  "updated_at": "2026-09-25T10:00:00Z",
  "secret": "whsec_5Hk9pQ2rT7vX1mN4bC8dF3gJ6"
}
```
{% endcode %}

The `addresses` you send are kept in the webhook's own address list, which the filter references through `list_refs`. The field names and every other option are in [Triggers and Filters](triggers-and-filters.md) and the [API Reference](api-reference/create-webhook.md).

### 3. Save the signing secret

The signing secret (`whsec_…`) is shown **only once**: in the dashboard in the **Your signing secret** window right after creation (copy it, then click **I saved it**), and in the API in the `secret` field of the `201` answer. No other screen or endpoint returns it.

Store it where your endpoint can read it, for example in the `GETBLOCK_WEBHOOK_SECRET` environment variable. If you lose it, rotate it — see [Rotating the secret](verifying-signatures.md#rotating-the-secret).

### 4. Send a test event

A test event is a delivery of the webhook's own event type with sample mainnet data, signed with the webhook's secret and sent to its URL like a live event.

* **Dashboard:** open the webhook and click **Send test event**.
* **API:** `POST /api/v1/webhooks/{id}/test`. The answer is `202` with `{"event_id": "…", "status": "queued"}`.

Test events are free and are sent only for an `active` webhook. Their `eventId` contains `:test:`. See [Test deliveries](delivery-format.md#test-deliveries).

### 5. Verify and acknowledge

A minimal handler verifies the signature over the raw body, de-duplicates on `eventId` + phase, answers `200` at once and does the real work afterwards. It imports `verifySignature` / `verify_signature` from the recipe file you save from [Verifying Signatures → Recipes](verifying-signatures.md#recipes) (`verify.mjs` or `verify.py`, next to the handler).

{% tabs %}
{% tab title="Node.js (Express)" %}
{% code overflow="wrap" %}
```javascript
// npm install express
import express from "express";
import { verifySignature } from "./verify.mjs"; // the recipe from Verifying Signatures

const app = express();
const secrets = [process.env.GETBLOCK_WEBHOOK_SECRET];
const seen = new Set(); // use your database in production

app.post("/getblock", express.raw({ type: "application/json" }), (req, res) => {
  // req.body is a Buffer with the exact bytes GetBlock signed
  const check = verifySignature(req.get("X-GetBlock-Signature") || "", req.body, secrets);
  if (!check.ok) return res.status(401).send(check.reason);

  const event = JSON.parse(req.body.toString("utf8"));
  const phase = event.removed ? "removed" : event.confirmed ? "confirmed" : "unconfirmed";
  const key = `${event.eventId}|${phase}`;

  res.sendStatus(200); // answer first: GetBlock waits at most 15 seconds

  if (seen.has(key)) return; // a repeat of a delivery you already have
  seen.add(key);
  setImmediate(() => handleEvent(event));
});

function handleEvent(event) {
  console.log(event.type, event.eventId, { confirmed: event.confirmed, removed: event.removed });
}

app.listen(3000);
```
{% endcode %}
{% endtab %}

{% tab title="Python (Flask)" %}
{% code overflow="wrap" %}
```python
# pip install flask
import json
import os
import threading

from flask import Flask, request

from verify import verify_signature  # the recipe from Verifying Signatures

app = Flask(__name__)
SECRETS = [os.environ["GETBLOCK_WEBHOOK_SECRET"]]
seen = set()  # use your database in production


def handle_event(event):
    print(event["type"], event["eventId"], event["confirmed"], event["removed"])


@app.post("/getblock")
def getblock_webhook():
    body = request.get_data()  # the exact bytes GetBlock signed
    ok, reason = verify_signature(request.headers.get("X-GetBlock-Signature", ""), body, SECRETS)
    if not ok:
        return reason, 401

    event = json.loads(body)
    phase = "removed" if event["removed"] else "confirmed" if event["confirmed"] else "unconfirmed"
    key = (event["eventId"], phase)
    if key not in seen:  # skip a repeat of a delivery you already have
        seen.add(key)
        # answer first, work after; use a task queue in production
        threading.Thread(target=handle_event, args=(event,)).start()
    return "", 200
```
{% endcode %}
{% endtab %}
{% endtabs %}

Send another test event: your handler should print it and answer `200`. The webhook's stats (`GET /api/v1/webhooks/{id}/stats` or the **Stats** tab) then show it in `delivered_total`.

### Production checklist

* [ ] Verify the signature of every request — do not rely on IP addresses.
* [ ] Answer `2xx` within 15 seconds and process the event asynchronously.
* [ ] De-duplicate on `eventId` + phase: delivery is at least once.
* [ ] Handle `"removed": true` by undoing what you did for that `eventId`.
* [ ] Watch for the `paused_auto` and `paused_no_balance` statuses and resume the webhook once the cause is fixed — nothing resumes it automatically.
* [ ] Keep your CU balance above zero.
* [ ] Rotate the secret with the 24-hour overlap: accept the old and the new secret until the old one expires.
* [ ] Keep the endpoint available: there is no replay, and an event is given up after 8 attempts over about 52 minutes.
