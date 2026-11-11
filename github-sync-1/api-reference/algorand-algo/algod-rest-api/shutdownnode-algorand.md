---
description: >-
  Example code for the ShutdownNode REST method. Complete guide on how to use
  ShutdownNode REST in GetBlock Web3 documentation.
---

# ShutdownNode - Algorand

Special management endpoint to shut down the node. Optionally provide a timeout parameter to indicate that the node should begin shutting down after a number of seconds. This endpoint is deprecated; use `POST /v2/node/shutdown` instead.

{% hint style="warning" %}
This is a node-administration, participation, or diagnostic endpoint and is not served on GetBlock shared endpoints. It requires a Dedicated Node.
{% endhint %}

## Endpoint

```http
POST /v2/shutdown
```

## Query Parameters

| Parameter | Type    | Required | Description |
| --------- | ------- | -------- | ----------- |
| timeout   | integer | Optional | —           |

## Example

{% code overflow="wrap" %}
```bash
export ALGO_ALGOD=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl --location --request POST "${ALGO_ALGOD}v2/shutdown" \
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
