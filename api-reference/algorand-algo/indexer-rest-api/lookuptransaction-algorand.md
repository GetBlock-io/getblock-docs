---
description: >-
  Example code for the lookupTransaction REST method. Complete guide on how to
  use lookupTransaction REST in GetBlock Web3 documentation.
---

# lookupTransaction - Algorand

Lookup a single transaction.

## Endpoint

```http
GET /v2/transactions/{txid}
```

## Path Parameters

| Parameter | Type   | Required | Description |
| --------- | ------ | -------- | ----------- |
| txid      | string | Yes      | —           |

## Example

{% code overflow="wrap" %}
```bash
export ALGO_INDEXER=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_INDEXER}v2/transactions/REPLACE_TXID"
```
{% endcode %}

## Response

Returns `application/json`. Top-level fields:

| Field         | Type    | Description                                                                                                                                        |
| ------------- | ------- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| current-round | integer | Round at which the results were computed.                                                                                                          |
| transaction   | object  | Contains all fields common to all transactions and serves as an envelope to all transactions type. Represents both regular and inner transactions. |

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400                       | Bad request   | A parameter was missing or malformed              |
| 404                       | Not found     | The requested resource does not exist             |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
