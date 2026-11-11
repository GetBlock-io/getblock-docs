---
description: >-
  Example code for the axelar/evm/v1beta1/gateway_address REST method. Complete
  guide on how to use axelar/evm/v1beta1/gateway_address REST method in GetBlock
  Web3 documentation.
---

# /axelar/evm/v1beta1/gateway\_address - Axelar

Returns the address of the Axelar Gateway contract deployed on a given EVM chain. The gateway is the on-chain entry point for cross-chain calls and transfers.

## Endpoint

```http
GET /axelar/evm/v1beta1/gateway_address
```

## Query Parameters

| Parameter | Type   | Description    |
| --------- | ------ | -------------- |
| chain     | string | EVM chain name |

## Example

{% code overflow="wrap" %}
```bash
export AXELAR_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${AXELAR_REST}axelar/evm/v1beta1/gateway_address?chain=ethereum"
```
{% endcode %}

## Response

```json
{
    "address": "0x4F4495243837681061C4743b74B3eEdf548D56A5"
}
```

## Response Fields

| Field   | Type   | Description                                      |
| ------- | ------ | ------------------------------------------------ |
| address | string | Axelar Gateway contract address on the EVM chain |

## Use Cases

* **GMP**: Resolve the gateway to send cross-chain messages
* **Integrations**: Wire up an EVM chain's Axelar entry point

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A parameter was invalid                           |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
