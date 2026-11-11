---
description: >-
  Example code for the axelar/nexus/v1beta1/transfer_fee REST method. Complete
  guide on how to use axelar/nexus/v1beta1/transfer_fee REST method in GetBlock
  Web3 documentation.
---

# /axelar/nexus/v1beta1/transfer\_fee - Axelar

Returns the fee Axelar charges to move an amount of an asset from a source chain to a destination chain.

## Endpoint

```http
GET /axelar/nexus/v1beta1/transfer_fee
```

## Query Parameters

| Parameter          | Type   | Description                         |
| ------------------ | ------ | ----------------------------------- |
| source\_chain      | string | Source chain name                   |
| destination\_chain | string | Destination chain name              |
| amount             | string | Amount with denom, e.g. 1000000uaxl |

## Example

{% code overflow="wrap" %}
```bash
export AXELAR_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${AXELAR_REST}axelar/nexus/v1beta1/transfer_fee?source_chain=ethereum&destination_chain=osmosis&amount=1000000uaxl"
```
{% endcode %}

## Response

```json
{
    "fee": {
        "denom": "uaxl",
        "amount": "1000"
    }
}
```

## Response Fields

| Field | Type   | Description                     |
| ----- | ------ | ------------------------------- |
| fee   | object | Transfer fee as {denom, amount} |

## Use Cases

* **Fee Preview**: Show the cross-chain transfer fee
* **Routing**: Budget a cross-chain transfer

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A parameter was invalid                           |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
