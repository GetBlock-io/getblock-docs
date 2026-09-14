---
description: >-
  Example code for the GetSyncRound REST method. Complete guide on how to use
  GetSyncRound REST in GetBlock Web3 documentation.
---

# GetSyncRound - Algorand

Gets the minimum sync round for the ledger.

{% hint style="warning" %}
This is a node-administration, participation, or diagnostic endpoint and is not served on GetBlock shared endpoints. It requires a Dedicated Node.
{% endhint %}

## Endpoint

```http
GET /v2/ledger/sync
```

## Example

{% code overflow="wrap" %}
```bash
export ALGO_ALGOD=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_ALGOD}v2/ledger/sync"
```
{% endcode %}

## Response

Returns `application/json`. Top-level fields:

| Field | Type    | Description                            |
| ----- | ------- | -------------------------------------- |
| round | integer | The minimum sync round for the ledger. |

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400                       | Bad request   | A parameter was missing or malformed              |
| 404                       | Not found     | The requested resource does not exist             |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
