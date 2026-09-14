---
description: >-
  Example code for the /health REST method. Complete guide on how to use the
  /health REST method in the GetBlock Web3 documentation.
---

# /health -

Returns the Indexer's health, including the round it has indexed up to and whether it is caught up with the algod node. Use it to confirm the index is current before searching.

## Endpoint

```http
GET /health
```

## Example

{% code overflow="wrap" %}
```bash
export ALGO_INDEXER=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_INDEXER}health"
```
{% endcode %}

## Response

```json
{
    "round": 35000000,
    "is-migrating": false,
    "db-available": true,
    "message": "",
    "version": "3.0.0"
}
```

## Response Fields

| Field        | Type    | Description                         |
| ------------ | ------- | ----------------------------------- |
| round        | integer | Round the Indexer has indexed up to |
| db-available | boolean | Whether the database is available   |
| is-migrating | boolean | Whether the index is migrating      |

## Use Cases

* **Readiness**: Confirm the index is current before searching
* **Monitoring**: Track indexer lag
* **Diagnostics**: Report indexer version and health

## Error Handling

| Error                     | Message        | Description                                       |
| ------------------------- | -------------- | ------------------------------------------------- |
| 500 / Internal            | Internal error | The Indexer is unavailable                        |
| 403 / RBAC: access denied | Access denied  | The GetBlock access token is missing or incorrect |
