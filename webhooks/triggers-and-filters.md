---
description: >-
  What fires a GetBlock webhook delivery: the three triggers, the unconfirmed
  and confirmed phases, the filter tree, and named address lists.
---

# Triggers and Filters

A webhook has a **trigger** — the kind of event it reacts to — and a **filter** that says which of those events you want: which addresses, contracts or topics. GetBlock delivers an event only when it matches both.

### Triggers

| `trigger_type` | Fires on | `phases` | Payload |
| --- | --- | --- | --- |
| `address_activity` | Native ETH or token activity of a watched address | `unconfirmed`, `confirmed` — one or both | `raw` and `block`; `decoded` for ERC-20 `Transfer` and `Approval` |
| `log_event` | A contract log matched by contract and/or topic filters | `unconfirmed`, `confirmed` — one or both | `raw` and `block`; `decoded` for ERC-20 `Transfer` and `Approval` |
| `tx_confirmation` | A transaction matched by the filter (for example, sent from or to a watched address) reaching `confirm_depth` blocks | Required, but ignored: the confirmation itself is the event | `raw` is the transaction; `block` is the chain head that reached the depth |

For `tx_confirmation`, `raw` and `eventId` are those of the block the transaction was mined in, while `block` names the newer block at which it reached the depth. See [Delivery Format](delivery-format.md) for the payload of each trigger.

### Phases and confirmation depth

* **`unconfirmed`** — the event is delivered as soon as it appears in a new block. A chain reorganization can still remove it.
* **`confirmed`** — the event is delivered again, with `"confirmed": true`, once the chain has grown `confirm_depth` blocks past the block that holds it.

A webhook with both phases gets **two deliveries per event**, and each is billed. `confirm_depth` is between 2 and 96 on Ethereum; leave it out or send `null` for the default, 96.

Reorg corrections (`"removed": true`) are delivered with the unconfirmed phase only. If you subscribe to `confirmed` alone, you are not told about a reorganization that happens after your confirmed delivery. See [Handling Chain Reorganizations](handling-chain-reorganizations.md#which-phase-to-subscribe-to).

{% hint style="info" %}
`tx_confirmation` still needs a non-empty `phases` list when you create the webhook (an empty one is refused with `400 invalid_phases`), but the value does not change what is delivered. Send `["confirmed"]`.
{% endhint %}

### Filters

The `filters` field is a tree. Every node is exactly one of:

* `{"all": [node, …]}` — every child must match;
* `{"any": [node, …]}` — at least one child must match;
* `{"not": node}` — the child must not match;
* a leaf.

There are three leaf types:

| Leaf | Fields | Matches |
| --- | --- | --- |
| `address` | `dir`: `in`, `out` or `any`; `in`: addresses (20-byte hex); `list_refs`: address list ids (`al_…`) | Activity to (`in`), from (`out`) or to or from (`any`) the addresses |
| `contract` | `in`: contract addresses (20-byte hex) | Logs emitted by these contracts |
| `topic` | `position`: 0 to 3; `in`: topics (32-byte hex) | Logs whose topic at that position is one of these values; position 0 is the event signature |

```json
{ "type": "address", "dir": "in", "in": ["0x742d35cc6634c0532925a3b844bc454e4438f44e"] }
```

Rules:

* The tree is at most 8 levels deep and has at most 256 nodes. At most 1,000 addresses can be written into the tree itself; use [address lists](#address-lists) for more.
* The trigger needs a matching leaf: `address_activity` needs an `address` leaf, `log_event` a `contract` or `topic` leaf. `tx_confirmation` accepts any leaf. A mismatch is refused with `400 filter_trigger_mismatch`.
* An unknown key is refused: `filter_invalid_leaf` inside a leaf, `filter_malformed` next to `all`, `any` or `not`.
* Addresses may be sent in any case, EIP-55 checksummed included, and are returned in lower case.
* The `abi` and `amount` leaves are not supported in this version.

#### Shortcuts for addresses and lists

Instead of writing an `address` leaf yourself, you can send two top-level fields when you create or update a webhook:

* `addresses` — addresses to watch in both directions. They are stored in the webhook's own address list, which you read with `GET /api/v1/webhooks/{id}/addresses`.
* `list_refs` — ids of your named [address lists](#address-lists) to watch in both directions.

GetBlock turns them into one `address` leaf with `"dir": "any"`. If you also send `filters`, the result is `{"all": [<your tree>, <that leaf>]}`. In the webhook returned by the API, that leaf carries `"src": "sugar"`. The marker is reserved: **remove that leaf before you send the tree back in `filters`**, or the request is refused with `400 filter_reserved_field`.

### Examples

{% tabs %}
{% tab title="Wallet activity" %}
Incoming and outgoing activity of two wallets, both phases:

{% code overflow="wrap" %}
```json
{
  "name": "hot-wallets",
  "chain": "eth",
  "network": "mainnet",
  "trigger_type": "address_activity",
  "phases": ["unconfirmed", "confirmed"],
  "target_url": "https://hooks.example.io/getblock",
  "addresses": [
    "0x742d35cc6634c0532925a3b844bc454e4438f44e",
    "0x2f1528f344f6410d361654d22a47ef121a60c938"
  ]
}
```
{% endcode %}
{% endtab %}

{% tab title="USDT transfers" %}
Every ERC-20 `Transfer` of USDT: the contract and the `Transfer` signature in topic position 0:

{% code overflow="wrap" %}
```json
{
  "name": "usdt-transfers",
  "chain": "eth",
  "network": "mainnet",
  "trigger_type": "log_event",
  "phases": ["unconfirmed", "confirmed"],
  "target_url": "https://hooks.example.io/getblock",
  "filters": {
    "all": [
      { "type": "contract", "in": ["0xdac17f958d2ee523a2206206994597c13d831ec7"] },
      { "type": "topic", "position": 0, "in": ["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"] }
    ]
  }
}
```
{% endcode %}
{% endtab %}

{% tab title="Transaction confirmations" %}
A notification when a transaction of the addresses in a named list is 12 blocks deep:

{% code overflow="wrap" %}
```json
{
  "name": "deposit-confirmations",
  "chain": "eth",
  "network": "mainnet",
  "trigger_type": "tx_confirmation",
  "phases": ["confirmed"],
  "confirm_depth": 12,
  "target_url": "https://hooks.example.io/getblock",
  "list_refs": ["al_7hK2mPq9Wx3Z"]
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

Send any of these bodies to `POST /api/v1/webhooks` — see [Create a webhook](api-reference/create-webhook.md).

### Address lists

A named address list (`al_…`) holds a set of addresses that any number of webhooks reference through `list_refs`. Changing the list changes what all of them match, without editing the webhooks.

* A list holds up to 100,000 addresses. An account can have 50 lists, and a webhook can reference 10.
* A request body is limited to about 5 MiB: 100,000 addresses (about 4.6 MB) fit in one request; larger sets go in several.
* A list that a webhook still references cannot be deleted (`409 list_in_use`): remove it from the webhook's `list_refs`, or delete the webhook, first.
* Every address of a list counts towards the plan limits of every webhook that references it. See [Plan limits](pricing-and-limits.md#plan-limits).

In the dashboard, lists are on the **Address lists** tab, where you can also import a `.txt` or `.csv` file. The endpoints are in the [API Reference](api-reference/README.md#address-lists).

### Changing a webhook

`PATCH /api/v1/webhooks/{id}` changes the fields you send and leaves the rest as they are.

* `chain`, `network` and `trigger_type` cannot be changed: sending them is refused with `400 malformed_body`. Create a new webhook instead.
* `addresses` and `list_refs` **replace** the current values; `[]` clears them.
* `"name": null` removes the name; `"confirm_depth": null` resets the depth to the default.
* A change that would leave the webhook with no condition at all is refused with `400 filter_would_be_empty`; send `filters` in the same request.
* A change applies to the next event at once. Retries that are already scheduled may use the previous settings for up to 60 seconds.

{% hint style="info" %}
The API also accepts a `batching` object. It is stored, but has no effect in this version: every request carries one event.
{% endhint %}
