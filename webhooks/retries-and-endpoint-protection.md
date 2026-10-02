---
description: >-
  How GetBlock webhooks treat your endpoint's answers: the retry schedule,
  concurrency and rate limits, the circuit breaker, auto-pause, target URL
  rules, delivery source addresses and the delivery history.
---

# Retries and Endpoint Protection

### Outcomes

| What your endpoint does | Outcome |
| --- | --- |
| Answers `2xx` within 15 seconds | Delivered |
| Answers anything else — `3xx`, any `4xx`, `5xx` | Failed attempt |
| Cannot be reached: a DNS, connection or TLS error, or the host resolves to a private or reserved address | Failed attempt |
| Does not connect within 5 seconds or does not answer within 15 seconds | Failed attempt |

Every failed attempt counts the same towards the circuit breaker and auto-pause below. No status code is treated as permanent: a `4xx` is retried like a `5xx`.

### Retry schedule

An event has **8 attempts**. The wait before each next attempt:

| After attempt | 1 | 2 | 3 | 4 | 5 | 6 | 7 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Wait | 5 s | 15 s | 30 s | 1 min | 5 min | 15 min | 30 min |

The last attempt comes about **52 minutes** after the first. If it fails too, the event is recorded as failed (visible in the [delivery history](#delivery-history-and-stats) with its last status or error) and **is not sent again: there is no replay.** An endpoint that is down for longer than that loses the events of that time.

Changes to a webhook — a new URL, a rotated secret, a pause — apply to the next event at once; retries already scheduled may use the previous settings for up to 60 seconds. A pause also stops matching at once, but a confirmed-phase delivery (`"confirmed": true`, or a `tx_confirmation`) of an event matched **before** the pause may still arrive for up to about 60 seconds after it.

### Endpoint protection

These rules protect your endpoint and everyone else's; you may notice them.

* **Concurrency:** at most **10** requests to one endpoint URL at a time. A request over the limit waits — up to 10 minutes in total, then it spends an attempt.
* **Request rate per host:** at most **200 requests per second** to one host, shared by all webhooks of all customers that point at that host. Waiting for the rate is not a failure — up to 10 minutes in total, then it spends an attempt.
* **Circuit breaker:** more than 90 % failed attempts over 2 minutes, with at least 20 attempts, stops requests to that endpoint URL for 5 minutes; then one probe request decides whether it reopens. While it is open, attempts are spent without a request. They are not billed, but they count in `failed_total` of the webhook's stats.
* **Auto-pause:** when every attempt of a webhook has failed for at least 30 minutes, with at least 20 attempts, the webhook becomes `paused_auto` (**Paused automatically** in the dashboard). Its queued events are dropped and recorded as `auto_paused`. It stays paused until you resume it: there is no automatic resume and no e-mail notification.
* **Loss window:** while a webhook is paused — by you, automatically, by plan or for balance — events are **not matched at all**. They are not queued and are never delivered later. The loss lasts until you resume.
* A routine secret rotation never causes failures by itself (both secrets sign for 24 hours), so it cannot trigger auto-pause.

Test deliveries count like any other delivery for the circuit breaker and auto-pause.

### Target URL rules

The URL is checked when you create a webhook and on every update that sends a `target_url`. Each rule has its own `400` code:

| Code | Refused |
| --- | --- |
| `target_url_required` | Empty |
| `target_url_malformed` | Not a URL, or an invalid host name |
| `target_url_must_be_https` | Any scheme but `https` |
| `target_url_has_userinfo` | `user:pass@` |
| `target_url_has_fragment` | `#…` |
| `target_url_no_host` | No host |
| `target_url_internal_address` | A literal IP that is loopback, private (RFC 1918, `fc00::/7`), link-local (incl. `169.254.169.254`), multicast, unspecified, `100.64.0.0/10`, `192.0.0.0/24`, `198.18.0.0/15`, `240.0.0.0/4` (incl. broadcast), or documentation space (`192.0.2.0/24`, `198.51.100.0/24`, `203.0.113.0/24`, `2001:db8::/32`) |
| `target_url_reserved_host` | `localhost`, `.local`, `.internal`, `.test`, `.invalid`, `.example`, `.home.arpa`, `.onion` (and names under them), any single-label name |
| `target_url_not_public_domain` | A name without a registrable label above an ICANN public suffix (`example.notatld`, `co.uk`), or a suffix itself (`github.io`) |

* Names under a hosting suffix are accepted: `foo.github.io`, `app.herokuapp.com`.
* The URL is stored in canonical form: scheme and host in lower case, a trailing dot removed, an international host name in punycode. Compare against what the API returns.
* No DNS lookup happens when you save the URL. At delivery time the host is resolved, and a private or reserved address is refused — that attempt fails.

### Delivery source addresses

GetBlock does **not** publish a list of delivery IP addresses. Deliveries come from GetBlock infrastructure in the EU, and the addresses may change without notice. **Verify the signature on every request — it is the only reliable check of origin** (see [Verifying Signatures](verifying-signatures.md)). An IP allowlist on your side is not recommended: it can start rejecting legitimate deliveries at any time.

### Delivery history and stats

The **Deliveries** and **Stats** tabs of a webhook in the dashboard show the same data as these two endpoints.

**`GET /api/v1/webhooks/{id}/deliveries`** lists **failed** attempts only, newest first. Under a long failure it keeps only a sample: about one row a second per webhook after the first 20. Query parameters:

* `from`, `to` — RFC 3339 times; `from` is inclusive, `to` exclusive;
* `status` — `retrying` (will be retried) or `failed_terminal` (given up), repeatable or comma-separated. `delivered` and `replayed` are accepted but never match: successful attempts, test deliveries included, are not listed, and nothing is replayed;
* `limit` — page size, 100 by default, at most 999; `cursor` — the `next_cursor` of the previous page.

{% code overflow="wrap" %}
```json
{
  "items": [
    {
      "event_id": "eth:mainnet:receipts:0x3805a4c919e7023ab2dc9be9b9bdf2bc44e0c157028948967c5f5f6f227baa64:0x1b6e7b7715daac0582a3039eed9b453d6bf520765b9dc4da77a4a83d457c4083:0",
      "attempt": 2,
      "status": "retrying",
      "http_status": 503,
      "latency_ms": 1200,
      "delivered_at": "2026-09-25T10:00:20Z"
    }
  ],
  "next_cursor": "…"
}
```
{% endcode %}

`http_status` is `null` when no response was received; `error`, when present, says why the attempt failed; `delivered_at` is when the attempt was made.

**`GET /api/v1/webhooks/{id}/stats`** has the exact counts:

* `delivered_total` — successful attempts;
* `failed_total` — failed attempts, each retry counted, including attempts spent without a request while the circuit breaker is open;
* `rate_limited_total` — events dropped above your plan's events-per-second limit (see [Plan limits](pricing-and-limits.md#plan-limits)).

`delivered_total` and `failed_total` include test deliveries. The answer also has `last_delivered_at`, `updated_at` and, when available, `rates` — delivered and failed counts over the last minute, 5 minutes and hour (`window_60s`, `window_300s`, `window_3600s`); `rates` is `null` and `rates_available` is `false` when they are temporarily unavailable. Totals update with a delay of a few seconds.
