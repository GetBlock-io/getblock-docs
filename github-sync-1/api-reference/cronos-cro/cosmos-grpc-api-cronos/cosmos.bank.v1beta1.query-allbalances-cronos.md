---
description: >-
  Example code for the cosmos.bank.v1beta1.Query/AllBalances gRPC method.
  Complete guide on how to use cosmos.bank.v1beta1.Query/AllBalances gRPC method
  in GetBlock Web3 documentation.
---

# cosmos.bank.v1beta1.Query/AllBalances - Cronos

Returns every coin balance held by an address, paginated. The gRPC equivalent of the REST balances query.

## Service Method

```
cosmos.bank.v1beta1.Query/AllBalances
```

## Message Fields

| Field      | Type   | Description                      |
| ---------- | ------ | -------------------------------- |
| address    | string | Bech32 address (crc1…)           |
| pagination | object | Optional pagination (limit, key) |

## Example

{% code overflow="wrap" %}
```bash
grpcurl \
  -H "x-api-key: <ACCESS-TOKEN>" \
  -d '{
    "address": "crc1p9zq0lr6h6n5s2wk9m3v7t4x8c6b2d0f1g3h5j"
}' \
  shared.eu-central-1.getblock.io:443 \
  cosmos.bank.v1beta1.Query/AllBalances
```
{% endcode %}

## Response

```json
{
    "balances": [
        {
            "denom": "basecro",
            "amount": "1000000000000000000"
        }
    ],
    "pagination": {
        "total": "1"
    }
}
```

## Response Fields

| Field      | Type   | Description                          |
| ---------- | ------ | ------------------------------------ |
| balances   | array  | All coin balances as {denom, amount} |
| pagination | object | Pagination cursor                    |

## Use Cases

* **Portfolio**: Enumerate all holdings
* **Backends**: Typed multi-denom reads
* **Indexing**: Snapshot balances

## Error Handling

| Error                     | Message         | Description                                       |
| ------------------------- | --------------- | ------------------------------------------------- |
| InvalidArgument           | Invalid address | The address is not valid bech32                   |
| 403 / RBAC: access denied | Access denied   | The GetBlock access token is missing or incorrect |
