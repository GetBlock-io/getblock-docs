---
description: >-
  Example code for the UnsetSyncRound REST method. Complete guide on how to use
  UnsetSyncRound REST in GetBlock Web3 documentation.
---

# UnsetSyncRound - Algorand

Unset the ledger sync round.

{% hint style="warning" %}
This is a node-administration, participation, or diagnostic endpoint and is not served on GetBlock shared endpoints. It requires a Dedicated Node.
{% endhint %}

## Endpoint

```http
DELETE /v2/ledger/sync
```

## Example

{% code overflow="wrap" %}
```bash
export ALGO_ALGOD=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl --location --request DELETE "${ALGO_ALGOD}v2/ledger/sync" \
--header 'Content-Type: application/json' \
--data-binary '<request body>'
```
{% endcode %}

## Response

Returns `application/json`. Top-level fields:

| Field  | Type             | Description   |
| ------ | ---------------- | ------------- |
| (body) | application/json | Response body |

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400                       | Bad request   | A parameter was missing or malformed              |
| 404                       | Not found     | The requested resource does not exist             |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
