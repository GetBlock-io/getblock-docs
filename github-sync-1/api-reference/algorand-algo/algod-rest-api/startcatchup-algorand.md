---
description: >-
  Example code for the StartCatchup REST method. Complete guide on how to use
  StartCatchup REST in GetBlock Web3 documentation.
---

# StartCatchup - Algorand

Given a catchpoint, it starts catching up to this catchpoint.

{% hint style="warning" %}
This is a node-administration, participation, or diagnostic endpoint and is not served on GetBlock shared endpoints. It requires a Dedicated Node.
{% endhint %}

## Endpoint

```http
POST /v2/catchup/{catchpoint}
```

## Path Parameters

| Parameter  | Type   | Required | Description   |
| ---------- | ------ | -------- | ------------- |
| catchpoint | string | Yes      | A catch point |

## Query Parameters

| Parameter | Type    | Required | Description                                                                                                                                                      |
| --------- | ------- | -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| min       | integer | Optional | Specify the minimum number of blocks which the ledger must be advanced by in order to start the catchup. This is useful for simplifying tools which support fast |

## Example

{% code overflow="wrap" %}
```bash
export ALGO_ALGOD=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl --location --request POST "${ALGO_ALGOD}v2/catchup/REPLACE_CATCHPOINT" \
--header 'Content-Type: application/json' \
--data-binary '<request body>'
```
{% endcode %}

## Response

Returns `application/json`. Top-level fields:

| Field           | Type   | Description                   |
| --------------- | ------ | ----------------------------- |
| catchup-message | string | Catchup start response string |

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400                       | Bad request   | A parameter was missing or malformed              |
| 404                       | Not found     | The requested resource does not exist             |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
