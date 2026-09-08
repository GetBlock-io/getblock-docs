---
description: >-
  Example code for the /rest/block/notxdetails/{blockhash} REST method. Complete
  guide on how to use the /rest/block/notxdetails/{blockhash} REST method in the
  GetBlock Web3 documentation.
---

# /rest/block/notxdetails/{blockhash} - Dash

Returns a block with only its header fields and the list of transaction ids (no full transaction details), a lighter alternative to the full block endpoint.

## Endpoint

```http
GET /rest/block/notxdetails/{blockhash}.json
```

## Path Parameters

| Parameter | Type   | Description       |
| --------- | ------ | ----------------- |
| blockhash | string | Hash of the block |

## Example

{% code overflow="wrap" %}
```bash
export DASH_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${DASH_REST}rest/block/notxdetails/000000000000000abc12def34567890fedcba9876543210abcdef1234567890ff.json"
```
{% endcode %}

## Response

```json
{
    "hash": "000000000000000abc12def34567890fedcba9876543210abcdef1234567890ff",
    "height": 2100000,
    "time": 1730000000,
    "tx": [
        "3a1f9c2e7b4d8a05f6c1e3d9b2a4c6e8f0d1b3a5c7e9f2d4b6a8c0e1f3d5b7a9c"
    ]
}
```

## Response Fields

| Field  | Type   | Description          |
| ------ | ------ | -------------------- |
| tx     | array  | Transaction ids only |
| height | number | Block height         |

## Use Cases

* **Lightweight Indexing**: List txids without full bodies
* **Header Reads**: Read block metadata cheaply
* **Pagination**: Fetch txids then hydrate selectively

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 404 / Not Found           | Not found     | No block matches the hash                         |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
