---
description: >-
  Example code for the block by height JSON-RPC method. Complete guide on how to
  use block by height JSON-RPC in GetBlock Web3 documentation.
---

# block by height - Movement

Returns a block by its height, including its first and last transaction versions, timestamp, and optionally the full transactions it contains.

## Endpoint

```http
GET /v1/blocks/by_height/{block_height}
```

## Path Parameters

| Parameter     | Type   | Description  |
| ------------- | ------ | ------------ |
| block\_height | string | Block height |

## Query Parameters

| Parameter          | Type   | Description                                         |
| ------------------ | ------ | --------------------------------------------------- |
| with\_transactions | string | Include full transactions in the block (true/false) |

## Example

{% code overflow="wrap" %}
```bash
export MOVEMENT_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${MOVEMENT_REST}v1/blocks/by_height/120000000?with_transactions=false"
```
{% endcode %}

## Response

```json
{
    "block_height": "120000000",
    "block_hash": "0x7md...",
    "block_timestamp": "1736772000000000",
    "first_version": "285000000",
    "last_version": "285000003",
    "transactions": null
}
```

## Response Fields

| Field            | Type   | Description                            |
| ---------------- | ------ | -------------------------------------- |
| block\_height    | string | Block height                           |
| first\_version   | string | First transaction version in the block |
| last\_version    | string | Last transaction version in the block  |
| block\_timestamp | string | Block time (microseconds)              |

## Use Cases

* **Indexing**: Ingest blocks and their version ranges
* **Explorers**: Render block contents by height

## Error Handling

| Error                     | Message       | Description                                                 |
| ------------------------- | ------------- | ----------------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid                |
| 404 / Not found           | Not found     | The account, block, transaction, or resource does not exist |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect           |
