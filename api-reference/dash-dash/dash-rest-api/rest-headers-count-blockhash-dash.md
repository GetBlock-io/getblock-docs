---
description: >-
  Example code for the /rest/headers/{count}/{blockhash} REST method. Complete
  guide on how to use the /rest/headers/{count}/{blockhash} REST method in the
  GetBlock Web3 documentation.
---

# /rest/headers/{count}/{blockhash} - Dash

Returns up to `<count>` consecutive block headers starting from a given block hash, as a JSON array. Useful for syncing header chains.

## Endpoint

```http
GET /rest/headers/{count}/{blockhash}.json
```

## Path Parameters

| Parameter | Type   | Description                 |
| --------- | ------ | --------------------------- |
| count     | number | Number of headers to return |
| blockhash | string | Starting block hash         |

## Example

{% code overflow="wrap" %}
```bash
export DASH_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${DASH_REST}rest/headers/5/000000000000000abc12def34567890fedcba9876543210abcdef1234567890ff.json"
```
{% endcode %}

## Response

```json
[
    {
        "hash": "000000000000000abc12def34567890fedcba9876543210abcdef1234567890ff",
        "height": 2100000,
        "time": 1730000000,
        "previousblockhash": "0x..."
    }
]
```

## Response Fields

| Field | Type  | Description                                      |
| ----- | ----- | ------------------------------------------------ |
| \[]   | array | Consecutive block headers from the starting hash |

## Use Cases

* **Header Sync**: Walk the header chain in batches
* **Light Clients**: Fetch headers without bodies
* **Verification**: Validate the header chain

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 404 / Not Found           | Not found     | The starting hash is unknown                      |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
