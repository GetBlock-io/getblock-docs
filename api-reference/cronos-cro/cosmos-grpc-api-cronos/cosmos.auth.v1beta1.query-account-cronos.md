# cosmos.auth.v1beta1.Query/Account - Cronos

Returns the account at a bech32 address, including account number and sequence, as a typed protobuf response. The gRPC equivalent of the REST auth account query.

## Service Method

```
cosmos.auth.v1beta1.Query/Account
```

## Message Fields

| Field   | Type   | Description                    |
| ------- | ------ | ------------------------------ |
| address | string | Bech32 account address (crc1…) |

## Example

{% code overflow="wrap" %}
```bash
grpcurl \
  -H "x-api-key: <ACCESS-TOKEN>" \
  -d '{
    "address": "crc1p9zq0lr6h6n5s2wk9m3v7t4x8c6b2d0f1g3h5j"
}' \
  shared.eu-central-1.getblock.io:443 \
  cosmos.auth.v1beta1.Query/Account
```
{% endcode %}

## Response

```json
{
    "account": {
        "@type": "/ethermint.types.v1.EthAccount",
        "base_account": {
            "address": "crc1p9zq0lr6h6n5s2wk9m3v7t4x8c6b2d0f1g3h5j",
            "account_number": "12345",
            "sequence": "42"
        }
    }
}
```

## Response Fields

| Field                                 | Type   | Description             |
| ------------------------------------- | ------ | ----------------------- |
| account.base\_account.sequence        | string | Account nonce           |
| account.base\_account.account\_number | string | On-chain account number |

## Use Cases

* **Signing**: Read sequence before signing
* **Backends**: Typed account reads in a production service
* **Existence**: Confirm an account exists

## Error Handling

| Error                     | Message           | Description                                       |
| ------------------------- | ----------------- | ------------------------------------------------- |
| NotFound                  | Account not found | No account at the address                         |
| 403 / RBAC: access denied | Access denied     | The GetBlock access token is missing or incorrect |
