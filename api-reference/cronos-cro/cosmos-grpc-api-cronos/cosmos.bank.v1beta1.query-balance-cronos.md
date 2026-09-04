---
description: >-
  Example code for the cosmos.bank.v1beta1.Query/Balance gRPC method. Complete
  guide on how to use cosmos.bank.v1beta1.Query/Balance gRPC method in GetBlock
  Web3 documentation.
---

# cosmos.bank.v1beta1.Query/Balance - Cronos

Returns the balance of a single denom held by an address. The gRPC equivalent of the REST by-denom balance query.

## Service Method

```
cosmos.bank.v1beta1.Query/Balance
```

## Message Fields

| Field   | Type   | Description                      |
| ------- | ------ | -------------------------------- |
| address | string | Bech32 address (crc1…)           |
| denom   | string | Token denom, e.g. the base denom |

## Example

{% code overflow="wrap" %}
```bash
grpcurl \
  -H "x-api-key: <ACCESS-TOKEN>" \
  -d '{
    "address": "crc1p9zq0lr6h6n5s2wk9m3v7t4x8c6b2d0f1g3h5j",
    "denom": "basecro"
}' \
  shared.eu-central-1.getblock.io:443 \
  cosmos.bank.v1beta1.Query/Balance
```
{% endcode %}

## Response

```json
{
    "balance": {
        "denom": "basecro",
        "amount": "1000000000000000000"
    }
}
```

## Response Fields

| Field   | Type   | Description                                  |
| ------- | ------ | -------------------------------------------- |
| balance | object | Coin {denom, amount} for the requested denom |

## Use Cases

* **Balance Reads**: Fetch a specific token balance efficiently
* **Backends**: Typed balance reads without JSON parsing
* **Accounting**: Reconcile a single denom

## Error Handling

| Error                     | Message         | Description                                       |
| ------------------------- | --------------- | ------------------------------------------------- |
| InvalidArgument           | Invalid address | The address or denom is invalid                   |
| 403 / RBAC: access denied | Access denied   | The GetBlock access token is missing or incorrect |
