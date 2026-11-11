---
description: >-
  Example code for the GetPeers REST method. Complete guide on how to use
  GetPeers REST in GetBlock Web3 documentation.
---

# GetPeers - Algorand

Get information about connected peers.

## Endpoint

```http
GET /v2/node/peers
```

## Example

{% code overflow="wrap" %}
```bash
export ALGO_ALGOD=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_ALGOD}v2/node/peers"
```
{% endcode %}

## Response

Returns `application/json`. Top-level fields:

| Field | Type  | Description |
| ----- | ----- | ----------- |
| Peers | array | —           |

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400                       | Bad request   | A parameter was missing or malformed              |
| 404                       | Not found     | The requested resource does not exist             |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
