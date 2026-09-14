---
description: >-
  Example code for the AccountApplicationsInformation REST method. Complete
  guide on how to use AccountApplicationsInformation REST in GetBlock Web3
  documentation.
---

# AccountApplicationsInformation - Algorand

Lookup an account's application holdings (local state and params if the account is the creator).

## Endpoint

```http
GET /v2/accounts/{address}/applications
```

## Path Parameters

| Parameter | Type   | Required | Description            |
| --------- | ------ | -------- | ---------------------- |
| address   | string | Yes      | An account public key. |

## Query Parameters

| Parameter | Type    | Required | Description                                                                                                                                                      |
| --------- | ------- | -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| limit     | integer | Optional | Maximum number of results to return.                                                                                                                             |
| next      | string  | Optional | The next page of results. Use the next token provided by the previous results.                                                                                   |
| include   | array   | Optional | Include additional items in the response. Use `params` to include full application parameters (global state, schema, etc.). Multiple values can be comma-separat |

## Example

{% code overflow="wrap" %}
```bash
export ALGO_ALGOD=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_ALGOD}v2/accounts/REPLACE_ADDRESS/applications"
```
{% endcode %}

## Response

Returns `application/json`. Top-level fields:

| Field                 | Type    | Description                                                                                                                                                     |
| --------------------- | ------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| application-resources | array   | —                                                                                                                                                               |
| next-token            | string  | Used for pagination, when making another request provide this token with the next parameter. The next token is the next application ID to use as the pagination |
| round                 | integer | The round for which this information is relevant.                                                                                                               |

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400                       | Bad request   | A parameter was missing or malformed              |
| 404                       | Not found     | The requested resource does not exist             |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
