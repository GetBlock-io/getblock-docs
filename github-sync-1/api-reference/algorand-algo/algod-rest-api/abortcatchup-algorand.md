---
description: >-
  Example code for the AbortCatchup REST method. Complete guide on how to use
  AbortCatchup REST in GetBlock Web3 documentation.
---

# AbortCatchup - Algorand

Given a catchpoint, it aborts catching up to this catchpoint.

{% hint style="warning" %}
This is a node-administration, participation, or diagnostic endpoint and is not served on GetBlock shared endpoints. It requires a Dedicated Node.
{% endhint %}

## Endpoint

```http
DELETE /v2/catchup/{catchpoint}
```

## Path Parameters

| Parameter  | Type   | Required | Description   |
| ---------- | ------ | -------- | ------------- |
| catchpoint | string | Yes      | A catch point |

## Example

{% code overflow="wrap" %}
```bash
export ALGO_ALGOD=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl --location --request DELETE "${ALGO_ALGOD}v2/catchup/REPLACE_CATCHPOINT" \
--header 'Content-Type: application/json' \
--data-binary '<request body>'
```
{% endcode %}

## Response

Returns `application/json`. Top-level fields:

| Field           | Type   | Description                   |
| --------------- | ------ | ----------------------------- |
| catchup-message | string | Catchup abort response string |

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400                       | Bad request   | A parameter was missing or malformed              |
| 404                       | Not found     | The requested resource does not exist             |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
