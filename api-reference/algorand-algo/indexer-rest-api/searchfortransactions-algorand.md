---
description: >-
  Example code for the searchForTransactions REST method. Complete guide on how
  to use searchForTransactions REST in GetBlock Web3 documentation.
---

# searchForTransactions - Algorand

Search for transactions. Transactions are returned oldest to newest unless the address parameter is used, in which case results are returned newest to oldest.

## Endpoint

```http
GET /v2/transactions
```

## Query Parameters

| Parameter             | Type    | Required | Description                                                                                                                                                      |
| --------------------- | ------- | -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| limit                 | integer | Optional | Maximum number of results to return. There could be additional pages even if the limit is not reached.                                                           |
| next                  | string  | Optional | The next page of results. Use the next token provided by the previous results.                                                                                   |
| note-prefix           | string  | Optional | Specifies a prefix which must be contained in the note field.                                                                                                    |
| tx-type               | string  | Optional | —                                                                                                                                                                |
| sig-type              | string  | Optional | SigType filters just results using the specified type of signature:                                                                                              |
| group-id              | string  | Optional | Lookup transactions by group ID. This field must be base64-encoded, and afterwards, base64 characters that are URL-unsafe (i.e. =, /, +) must be URL-encoded     |
| txid                  | string  | Optional | Lookup the specific transaction by ID.                                                                                                                           |
| round                 | integer | Optional | Include results for the specified round.                                                                                                                         |
| min-round             | integer | Optional | Include results at or after the specified min-round.                                                                                                             |
| max-round             | integer | Optional | Include results at or before the specified max-round.                                                                                                            |
| asset-id              | integer | Optional | Asset ID                                                                                                                                                         |
| before-time           | string  | Optional | Include results before the given time. Must be an RFC 3339 formatted string.                                                                                     |
| after-time            | string  | Optional | Include results after the given time. Must be an RFC 3339 formatted string.                                                                                      |
| currency-greater-than | integer | Optional | Results should have an amount greater than this value. MicroAlgos are the default currency unless an asset-id is provided, in which case the asset will be used. |
| currency-less-than    | integer | Optional | Results should have an amount less than this value. MicroAlgos are the default currency unless an asset-id is provided, in which case the asset will be used.    |
| address               | string  | Optional | Only include transactions with this address in one of the transaction fields.                                                                                    |
| address-role          | string  | Optional | Combine with the address parameter to define what type of address to search for.                                                                                 |
| exclude-close-to      | boolean | Optional | Combine with address and address-role parameters to define what type of address to search for. The close to fields are normally treated as a receiver, if you wo |
| rekey-to              | boolean | Optional | Include results which include the rekey-to field.                                                                                                                |
| application-id        | integer | Optional | Application ID                                                                                                                                                   |

## Example

{% code overflow="wrap" %}
```bash
export ALGO_INDEXER=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_INDEXER}v2/transactions"
```
{% endcode %}

## Response

Returns `application/json`. Top-level fields:

| Field         | Type    | Description                                                                                  |
| ------------- | ------- | -------------------------------------------------------------------------------------------- |
| current-round | integer | Round at which the results were computed.                                                    |
| next-token    | string  | Used for pagination, when making another request provide this token with the next parameter. |
| transactions  | array   | —                                                                                            |

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400                       | Bad request   | A parameter was missing or malformed              |
| 404                       | Not found     | The requested resource does not exist             |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
