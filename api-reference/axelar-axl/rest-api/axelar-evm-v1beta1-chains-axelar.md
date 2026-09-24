---
description: >-
  Example code for the axelar/evm/v1beta1/chains REST method. Complete guide on
  how to use axelar/evm/v1beta1/chains REST method in GetBlock Web3
  documentation.
---

# axelar/evm/v1beta1/chains - Axelar

Returns the EVM chains registered with Axelar's EVM module, optionally filtered by status.

## Endpoint

```http
GET /axelar/evm/v1beta1/chains
```

## Query Parameters

| Parameter | Type   | Description            |
| --------- | ------ | ---------------------- |
| status    | string | Filter by chain status |

## Example

{% code overflow="wrap" %}
```bash
export AXELAR_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${AXELAR_REST}axelar/evm/v1beta1/chains"
```
{% endcode %}

## Response

```json
{
    "chains": [
        "Ethereum",
        "avalanche",
        "Polygon",
        "binance",
        "Fantom",
        "moonbeam"
    ]
}
```

## Response Fields

| Field  | Type  | Description                |
| ------ | ----- | -------------------------- |
| chains | array | Registered EVM chain names |

## Use Cases

* **EVM Routing**: List connected EVM chains
* **Gateway Discovery**: Enumerate EVM chains before querying gateways

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A parameter was invalid                           |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
