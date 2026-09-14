---
description: >-
  Example code for the /v2/assets REST method. Complete guide on how to use the
  /v2/assets REST method in the GetBlock Web3 documentation.
---

# /v2/assets - Algorand

Searches Algorand Standard Assets by name, unit name, creator, or asset id. Paginated.

## Endpoint

```http
GET /v2/assets
```

## Query Parameters

| Parameter | Type    | Description               |
| --------- | ------- | ------------------------- |
| name      | string  | Filter by asset name      |
| unit      | string  | Filter by unit name       |
| creator   | string  | Filter by creator address |
| limit     | integer | Max results               |

## Example

{% code overflow="wrap" %}
```bash
export ALGO_INDEXER=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_INDEXER}v2/assets"
```
{% endcode %}

## Response

```json
{
    "assets": [
        {
            "index": 31566704,
            "params": {
                "name": "USDC",
                "unit-name": "USDC",
                "decimals": 6,
                "total": 18446744073709551615,
                "creator": "XBYLS2E6YI6XXL5BWCAMOA4GTWHXWENZMX5UHXMRNWWUQ7BXCY5WC5TEPA"
            }
        }
    ],
    "current-round": 35000000,
    "next-token": "b..."
}
```

## Response Fields

| Field      | Type   | Description                         |
| ---------- | ------ | ----------------------------------- |
| assets     | array  | Matching ASAs with their parameters |
| next-token | string | Pagination token                    |

## Use Cases

* **Token Discovery**: Find an ASA by name or creator
* **Explorers**: Power an asset search box
* **Analytics**: Enumerate assets

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A filter parameter is invalid                     |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
