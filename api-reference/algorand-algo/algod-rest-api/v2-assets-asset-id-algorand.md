---
description: >-
  Example code for the /v2/assets/{asset-id} REST method. Complete guide on how
  to use the /v2/assets/{asset-id} REST method in the GetBlock Web3
  documentation.
---

# /v2/assets/{asset-id} - Algorand

Returns the parameters of an Algorand Standard Asset (ASA): its name, unit name, total supply, decimals, and the manager/reserve/freeze/clawback addresses.

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
export ALGO_ALGOD=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_ALGOD}v2/assets/31566704"
```
{% endcode %}

## Response

```json
{
    "index": 31566704,
    "params": {
        "name": "USDC",
        "unit-name": "USDC",
        "total": 18446744073709551615,
        "decimals": 6,
        "creator": "XBYLS2E6YI6XXL5BWCAMOA4GTWHXWENZMX5UHXMRNWWUQ7BXCY5WC5TEPA",
        "manager": "XBYLS2E6YI6XXL5BWCAMOA4GTWHXWENZMX5UHXMRNWWUQ7BXCY5WC5TEPA",
        "reserve": "XBYLS2E6YI6XXL5BWCAMOA4GTWHXWENZMX5UHXMRNWWUQ7BXCY5WC5TEPA",
        "freeze": "XBYLS2E6YI6XXL5BWCAMOA4GTWHXWENZMX5UHXMRNWWUQ7BXCY5WC5TEPA",
        "clawback": "XBYLS2E6YI6XXL5BWCAMOA4GTWHXWENZMX5UHXMRNWWUQ7BXCY5WC5TEPA",
        "default-frozen": false
    }
}
```

## Response Fields

| Field           | Type    | Description                    |
| --------------- | ------- | ------------------------------ |
| params.name     | string  | Asset name                     |
| params.total    | integer | Total units in circulation     |
| params.decimals | integer | Decimals for display           |
| params.creator  | string  | Account that created the asset |

## Use Cases

* **Token Metadata**: Read an ASA's name, supply, and decimals
* **Balance Formatting**: Use decimals to display holdings
* **Explorers**: Render asset pages

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 404 / Not found           | Not found     | No asset with that id                             |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
