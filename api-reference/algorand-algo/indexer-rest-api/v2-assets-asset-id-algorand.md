---
description: >-
  Example code for the /v2/assets/{asset-id} REST method. Complete guide on how
  to use the /v2/assets/{asset-id} REST method in the GetBlock Web3
  documentation.
---

# /v2/assets/{asset-id} - Algorand

Returns an ASA's parameters from the Indexer, including whether it has been deleted and the round the data applies to.

## Endpoint

```http
GET /v2/assets/{asset-id}
```

## Path Parameters

| Parameter | Type    | Description |
| --------- | ------- | ----------- |
| asset-id  | integer | Asset id    |

## Example

{% code overflow="wrap" %}
```bash
export ALGO_INDEXER=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_INDEXER}v2/assets/31566704"
```
{% endcode %}

## Response

```json
{
    "asset": {
        "index": 31566704,
        "params": {
            "name": "USDC",
            "unit-name": "USDC",
            "decimals": 6,
            "total": 18446744073709551615,
            "creator": "XBYLS2E6YI6XXL5BWCAMOA4GTWHXWENZMX5UHXMRNWWUQ7BXCY5WC5TEPA"
        },
        "deleted": false
    },
    "current-round": 35000000
}
```

## Response Fields

| Field         | Type    | Description                                         |
| ------------- | ------- | --------------------------------------------------- |
| asset.params  | object  | Asset parameters (name, total, decimals, addresses) |
| asset.deleted | boolean | Whether the asset has been destroyed                |

## Use Cases

* **Token Metadata**: Read an ASA's parameters
* **Explorers**: Render asset pages
* **Verification**: Confirm an asset's creator

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 404 / Not found           | Not found     | No asset matches the id                           |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
