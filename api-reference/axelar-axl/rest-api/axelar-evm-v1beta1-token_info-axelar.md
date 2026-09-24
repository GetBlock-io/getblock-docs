---
description: >-
  Example code for the axelar/evm/v1beta1/token_info REST method. Complete guide
  on how to use axelar/evm/v1beta1/token_info REST method in GetBlock Web3
  documentation.
---

# /axelar/evm/v1beta1/token\_info - Axelar

Returns the ERC-20 token information (address, symbol, decimals) for an Axelar asset on a given EVM chain.

## Endpoint

```http
GET /axelar/evm/v1beta1/token_info
```

## Query Parameters

| Parameter | Type   | Description                  |
| --------- | ------ | ---------------------------- |
| chain     | string | EVM chain name               |
| asset     | string | Axelar asset denom or symbol |

## Example

{% code overflow="wrap" %}
```bash
export AXELAR_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${AXELAR_REST}axelar/evm/v1beta1/token_info?chain=ethereum&asset=uaxl"
```
{% endcode %}

## Response

```json
{
    "asset": "uaxl",
    "details": {
        "token_name": "Axelar",
        "symbol": "AXL",
        "decimals": 6
    },
    "address": "0x467719aD09025FcC6cF6F8311755809d45a5E5f3",
    "confirmed": true,
    "is_external": false,
    "burner_code_hash": "0x..."
}
```

## Response Fields

| Field     | Type    | Description                               |
| --------- | ------- | ----------------------------------------- |
| address   | string  | ERC-20 token address on the EVM chain     |
| details   | object  | Token name, symbol, and decimals          |
| confirmed | boolean | Whether the token deployment is confirmed |

## Use Cases

* **Asset Mapping**: Resolve an asset's ERC-20 address
* **Wallets**: Display bridged token metadata

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A parameter was invalid                           |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
