---
description: >-
  Example code for the /v2/status REST method. Complete guide on how to use the
  /v2/status REST method in the GetBlock Web3 documentation.
---

# /v2/status - Algorand

Returns the node's current status: the last committed round, the time since the last round, catchup progress, and the consensus version. The primary way to read the chain tip and confirm the node is synced.

## Endpoint

```http
GET /v2/status
```

## Example

{% code overflow="wrap" %}
```bash
export ALGO_ALGOD=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_ALGOD}v2/status"
```
{% endcode %}

## Response

```json
{
    "last-round": 35000000,
    "last-version": "future",
    "next-version-round": 35000001,
    "time-since-last-round": 1200000000,
    "catchup-time": 0,
    "next-version-supported": true,
    "stopped-at-unsupported-round": false
}
```

## Response Fields

| Field                 | Type    | Description                                 |
| --------------------- | ------- | ------------------------------------------- |
| last-round            | integer | Latest committed round (block height)       |
| time-since-last-round | integer | Nanoseconds since the last round            |
| catchup-time          | integer | Catchup time in nanoseconds (0 when synced) |

## Use Cases

* **Chain Tip**: Read last-round as the current height
* **Sync Detection**: Confirm catchup-time is 0
* **Monitoring**: Track round production timing

## Error Handling

| Error                     | Message        | Description                                       |
| ------------------------- | -------------- | ------------------------------------------------- |
| 500 / Internal            | Internal error | The node failed to report status                  |
| 403 / RBAC: access denied | Access denied  | The GetBlock access token is missing or incorrect |
