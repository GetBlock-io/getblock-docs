# auth account

Returns the account at a bech32 address, including account number and sequence (nonce). The sequence is required to build a transaction.

## Endpoint

```http
GET /cosmos/auth/v1beta1/accounts/{address}
```

## Path Parameters

| Parameter | Type   | Description                      |
| --------- | ------ | -------------------------------- |
| address   | string | Bech32 account address (akash1…) |

## Example

{% code overflow="wrap" %}
```bash
export AKASH_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${AKASH_REST}cosmos/auth/v1beta1/accounts/akash1nl7dg3xj9j2y6q7z0v4w8c2b5d1f3g6h9k2m4p"
```
{% endcode %}

## Response

```json
{
    "account": {
        "@type": "/cosmos.auth.v1beta1.BaseAccount",
        "address": "akash1nl7dg3xj9j2y6q7z0v4w8c2b5d1f3g6h9k2m4p",
        "account_number": "12345",
        "sequence": "42"
    }
}
```

## Response Fields

| Field                   | Type   | Description             |
| ----------------------- | ------ | ----------------------- |
| account.account\_number | string | On-chain account number |
| account.sequence        | string | Account nonce           |

## Use Cases

* **Signing**: Read the sequence before signing
* **Existence**: Confirm an account exists
* **Wallets**: Populate account metadata

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A parameter was invalid                           |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
