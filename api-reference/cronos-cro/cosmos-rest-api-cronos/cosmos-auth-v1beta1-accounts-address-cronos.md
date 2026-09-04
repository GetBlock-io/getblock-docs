# /cosmos/auth/v1beta1/accounts/{address} - Cronos

Returns the account at a bech32 address, including its account number and sequence (nonce). The sequence is required to build and sign a transaction from the account.

## Endpoint

```http
GET /cosmos/auth/v1beta1/accounts/{address}
```

## Path Parameters

| Parameter | Type   | Description                    |
| --------- | ------ | ------------------------------ |
| address   | string | Bech32 account address (crc1…) |

## Example

{% code overflow="wrap" %}
```bash
export CRONOS_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${CRONOS_REST}cosmos/auth/v1beta1/accounts/crc1p9zq0lr6h6n5s2wk9m3v7t4x8c6b2d0f1g3h5j"
```
{% endcode %}

## Response

```json
{
    "account": {
        "@type": "/ethermint.types.v1.EthAccount",
        "base_account": {
            "address": "crc1p9zq0lr6h6n5s2wk9m3v7t4x8c6b2d0f1g3h5j",
            "pub_key": null,
            "account_number": "12345",
            "sequence": "42"
        }
    }
}
```

## Response Fields

| Field                                 | Type   | Description                                   |
| ------------------------------------- | ------ | --------------------------------------------- |
| account.base\_account.account\_number | string | On-chain account number                       |
| account.base\_account.sequence        | string | Account nonce for the next transaction        |
| account.@type                         | string | Account type (Ethermint EthAccount on Cronos) |

## Use Cases

* **Signing**: Read the sequence before building a transaction
* **Account Existence**: Confirm an address has been seen on-chain
* **Wallet Backends**: Populate account metadata

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 404 / not found           | Not found     | No account exists at the address                  |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
