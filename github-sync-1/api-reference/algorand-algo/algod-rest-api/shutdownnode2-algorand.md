---
description: >-
  Example code for the ShutdownNode2 REST method. Complete guide on how to use
  ShutdownNode2 REST in GetBlock Web3 documentation.
---

# ShutdownNode2 - Algorand

Special management endpoint to shut down the node. Optionally provide a timeout parameter to indicate that the node should begin shutting down after a number of seconds.

## Endpoint

```http
POST /v2/node/shutdown
```

## Query Parameters

| Parameter | Type    | Required | Description |
| --------- | ------- | -------- | ----------- |
| timeout   | integer | Optional | —           |

## Example

{% code overflow="wrap" %}
```bash
export ALGO_ALGOD=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl --location --request POST "${ALGO_ALGOD}v2/node/shutdown" \
--header 'Content-Type: application/json' \
--data-binary '<request body>'
```
{% endcode %}

## Response

Returns HTTP 200 on success.

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400                       | Bad request   | A parameter was missing or malformed              |
| 404                       | Not found     | The requested resource does not exist             |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
