---
description: >-
  Example code for the searchForBlockHeaders REST method. Complete guide on how
  to use searchForBlockHeaders REST in GetBlock Web3 documentation.
---

# searchForBlockHeaders - Algorand

Search for block headers. Block headers are returned in ascending round order. Transactions are not included in the output.

## Endpoint

```http
GET /v2/block-headers
```

## Query Parameters

| Parameter   | Type    | Required | Description                                                                                                                          |
| ----------- | ------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| limit       | integer | Optional | Maximum number of results to return. There could be additional pages even if the limit is not reached.                               |
| next        | string  | Optional | The next page of results. Use the next token provided by the previous results.                                                       |
| min-round   | integer | Optional | Include results at or after the specified min-round.                                                                                 |
| max-round   | integer | Optional | Include results at or before the specified max-round.                                                                                |
| before-time | string  | Optional | Include results before the given time. Must be an RFC 3339 formatted string.                                                         |
| after-time  | string  | Optional | Include results after the given time. Must be an RFC 3339 formatted string.                                                          |
| proposers   | array   | Optional | Accounts marked as proposer in the block header's participation updates. This parameter accepts a comma separated list of addresses. |
| expired     | array   | Optional | Accounts marked as expired in the block header's participation updates. This parameter accepts a comma separated list of addresses.  |
| absent      | array   | Optional | Accounts marked as absent in the block header's participation updates. This parameter accepts a comma separated list of addresses.   |

## Example

{% code overflow="wrap" %}
```bash
export ALGO_INDEXER=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_INDEXER}v2/block-headers"
```
{% endcode %}

## Response

Returns `application/json`. Top-level fields:

| Field         | Type    | Description                                                                                  |
| ------------- | ------- | -------------------------------------------------------------------------------------------- |
| blocks        | array   | —                                                                                            |
| current-round | integer | Round at which the results were computed.                                                    |
| next-token    | string  | Used for pagination, when making another request provide this token with the next parameter. |

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400                       | Bad request   | A parameter was missing or malformed              |
| 404                       | Not found     | The requested resource does not exist             |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
