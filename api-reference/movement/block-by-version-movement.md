---
description: >-
  Example code for the block by version JSON-RPC method. Complete guide on how
  to use block by version JSON-RPC in GetBlock Web3 documentation.
---

# block by version - Movement

Returns the block that contains a given ledger (transaction) version. Use it to map a transaction version back to its block.

## Endpoint

```http
GET /v1/blocks/by_version/{version}
```

## Path Parameters

| Parameter | Type   | Description                  |
| --------- | ------ | ---------------------------- |
| version   | string | Ledger (transaction) version |

## Query Parameters

| Parameter          | Type   | Description                            |
| ------------------ | ------ | -------------------------------------- |
| with\_transactions | string | Include full transactions (true/false) |

## Example

{% code overflow="wrap" %}
```bash
export MOVEMENT_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${MOVEMENT_REST}v1/blocks/by_version/285000000?with_transactions=false"
```
{% endcode %}

## Response

```json
{
    "block_height": "120000000",
    "block_hash": "0x7md...",
    "block_timestamp": "1736772000000000",
    "first_version": "285000000",
    "last_version": "285000003"
}
```

## Response Fields

| Field          | Type   | Description                                |
| -------------- | ------ | ------------------------------------------ |
| block\_height  | string | Height of the block containing the version |
| first\_version | string | First version in the block                 |
| last\_version  | string | Last version in the block                  |

## Use Cases

* **Version Mapping**: Resolve a transaction version to its block

## Error Handling

| Error                     | Message       | Description                                                 |
| ------------------------- | ------------- | ----------------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid                |
| 404 / Not found           | Not found     | The account, block, transaction, or resource does not exist |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect           |
