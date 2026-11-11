---
description: >-
  Example code for the GetApplicationBoxes REST method. Complete guide on how to
  use GetApplicationBoxes REST in GetBlock Web3 documentation.
---

# GetApplicationBoxes - Algorand

Given an application ID, return all box names. No particular ordering is guaranteed. Request fails when client or server-side configured limits prevent returning all box names.

Pagination mode is enabled when any of the following parameters are provided: `limit`, `next`, `prefix`, `include`, or `round`. In pagination mode box values can be requested and results are returned in sorted order.

To paginate: use the next-token from a previous response as the `next` parameter in the following request. Pin the `round` parameter to the round value from the first page's response to ensure consistent results across pages. The server enforces a per-response byte limit, so fewer results than `limit` may be returned even when more exist; the presence of `next-token` is the only reliable signal that more data is available.

## Endpoint

```http
GET /v2/applications/{application-id}/boxes
```

## Path Parameters

| Parameter      | Type    | Required | Description                |
| -------------- | ------- | -------- | -------------------------- |
| application-id | integer | Yes      | An application identifier. |

## Query Parameters

| Parameter | Type    | Required | Description                                                                                                                                                      |
| --------- | ------- | -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| max       | integer | Optional | Max number of box names to return. If max is not set, or max == 0, returns all box-names.                                                                        |
| limit     | integer | Optional | Maximum number of boxes to return per page.                                                                                                                      |
| next      | string  | Optional | A box name, in the goal app call arg form 'encoding:value', representing the earliest box name to include in results. Use the next-token from a previous respons |
| prefix    | string  | Optional | A box name prefix, in the goal app call arg form 'encoding:value', to filter results by. Only boxes whose names start with this prefix will be returned.         |
| include   | array   | Optional | Include additional items in the response. Use `values` to include box values. Multiple values can be comma-separated.                                            |
| round     | integer | Optional | Return box data from the given round. The round must be within the node's available range.                                                                       |

## Example

{% code overflow="wrap" %}
```bash
export ALGO_ALGOD=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_ALGOD}v2/applications/REPLACE_APPLICATION-ID/boxes"
```
{% endcode %}

## Response

Returns `application/json`. Top-level fields:

| Field      | Type    | Description                                                                                                                                                        |
| ---------- | ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| boxes      | array   | —                                                                                                                                                                  |
| next-token | string  | Used for pagination, when making another request provide this token with the `next` parameter. The next token is the box name to use as the pagination cursor, enc |
| round      | integer | The round for which this information is relevant.                                                                                                                  |

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400                       | Bad request   | A parameter was missing or malformed              |
| 404                       | Not found     | The requested resource does not exist             |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
