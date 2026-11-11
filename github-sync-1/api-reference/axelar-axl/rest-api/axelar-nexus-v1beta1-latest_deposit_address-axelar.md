---
description: >-
  Example code for the axelar/nexus/v1beta1/latest_deposit_address REST method.
  Complete guide on how to use axelar/nexus/v1beta1/latest_deposit_address REST
  method in GetBlock Web3 documentation.
---

# /axelar/nexus/v1beta1/latest\_deposit\_address - Axelar

Returns the latest generated deposit address for a cross-chain transfer to a recipient on a destination chain. Sending assets to this address bridges them to the recipient.

## Endpoint

```http
GET /axelar/nexus/v1beta1/latest_deposit_address
```

## Query Parameters

| Parameter        | Type   | Description                                |
| ---------------- | ------ | ------------------------------------------ |
| recipient\_addr  | string | Recipient address on the destination chain |
| recipient\_chain | string | Destination chain name                     |
| deposit\_chain   | string | Chain the deposit is made on               |

## Example

{% code overflow="wrap" %}
```bash
export AXELAR_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${AXELAR_REST}axelar/nexus/v1beta1/latest_deposit_address?recipient_addr=0xAbC...&recipient_chain=ethereum&deposit_chain=Axelarnet"
```
{% endcode %}

## Response

```json
{
    "address": "axelar1nl7dg3xj9j2y6q7z0v4w8c2b5d1f3g6h9k2m4p"
}
```

## Response Fields

| Field   | Type   | Description                               |
| ------- | ------ | ----------------------------------------- |
| address | string | Deposit address to send bridged assets to |

## Use Cases

* **Bridging**: Get a deposit address for a transfer
* **Wallets**: Show the bridge deposit target

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A parameter was invalid                           |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
