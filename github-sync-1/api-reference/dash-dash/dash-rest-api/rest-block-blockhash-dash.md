---
description: >-
  Example code for the /rest/block/{blockhash} REST method. Complete guide on
  how to use the /rest/block/{blockhash} REST method in the GetBlock Web3
  documentation.
---

# /rest/block/{blockhash} - Dash

Returns a full block, including its transactions, as JSON for a given block hash over the REST interface.

## Endpoint

```http
GET /rest/block/{blockhash}.json
```

## Path Parameters

| Parameter | Type   | Description       |
| --------- | ------ | ----------------- |
| blockhash | string | Hash of the block |

## Example

{% code overflow="wrap" %}
```bash
export DASH_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${DASH_REST}rest/block/000000000000000abc12def34567890fedcba9876543210abcdef1234567890ff.json"
```
{% endcode %}

## Response

```json
{
    "hash": "000000000000000abc12def34567890fedcba9876543210abcdef1234567890ff",
    "height": 2100000,
    "time": 1730000000,
    "chainlock": true,
    "tx": [
        {
            "txid": "3a1f9c2e7b4d8a05f6c1e3d9b2a4c6e8f0d1b3a5c7e9f2d4b6a8c0e1f3d5b7a9c"
        }
    ]
}
```

## Response Fields

| Field     | Type    | Description                    |
| --------- | ------- | ------------------------------ |
| height    | number  | Block height                   |
| tx        | array   | Full transactions in the block |
| chainlock | boolean | ChainLock protection status    |

## Use Cases

* **Block Reads**: Fetch a full block over REST
* **Indexing**: Ingest block + txs in one request
* **Explorers**: Render block pages

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 404 / Not Found           | Not found     | No block matches the hash                         |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
