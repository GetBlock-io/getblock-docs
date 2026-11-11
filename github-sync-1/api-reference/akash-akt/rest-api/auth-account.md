---
description: >-
  Example code for the cosmos/auth/v1beta1/accounts/{address} REST method.
  Complete guide on how to use cosmos/auth/v1beta1/accounts/{address} REST
  method in GetBlock Web3 documentation.
---

# /cosmos/auth/v1beta1/accounts/{address} - Akash

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

curl "${AKASH_REST}cosmos/auth/v1beta1/accounts/akash17xpfvakm2amg962yls6f84z3kell8c5lazw8j8"
```
{% endcode %}

## Response

```json
{
    "account": {
        "@type": "/cosmos.auth.v1beta1.ModuleAccount",
        "base_account": {
            "address": "akash17xpfvakm2amg962yls6f84z3kell8c5lazw8j8",
            "pub_key": null,
            "account_number": "220",
            "sequence": "0"
        },
        "name": "fee_collector",
        "permissions": []
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
