---
description: >-
  Example code for the AccountApplicationInformation REST method. Complete guide
  on how to use AccountApplicationInformation REST in GetBlock Web3
  documentation.
---

# AccountApplicationInformation - Algorand

Given a specific account public key and application ID, this call returns the account's application local state and global state (AppLocalState and AppParams, if either exists). Global state will only be returned if the provided address is the application's creator.

## Endpoint

```http
GET /v2/accounts/{address}/applications/{application-id}
```

## Path Parameters

| Parameter      | Type    | Required | Description                |
| -------------- | ------- | -------- | -------------------------- |
| address        | string  | Yes      | An account public key.     |
| application-id | integer | Yes      | An application identifier. |

## Query Parameters

| Parameter | Type   | Required | Description                                                                                               |
| --------- | ------ | -------- | --------------------------------------------------------------------------------------------------------- |
| format    | string | Optional | Configures whether the response object is JSON or MessagePack encoded. If not provided, defaults to JSON. |

## Example

{% code overflow="wrap" %}
```bash
export ALGO_ALGOD=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_ALGOD}v2/accounts/REPLACE_ADDRESS/applications/REPLACE_APPLICATION-ID"
```
{% endcode %}

## Response

Returns `application/json`. Top-level fields:

| Field           | Type    | Description                                                   |
| --------------- | ------- | ------------------------------------------------------------- |
| app-local-state | object  | Stores local state associated with an application.            |
| created-app     | object  | Stores the global information associated with an application. |
| round           | integer | The round for which this information is relevant.             |

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400                       | Bad request   | A parameter was missing or malformed              |
| 404                       | Not found     | The requested resource does not exist             |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
