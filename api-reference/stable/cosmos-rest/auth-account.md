---
description: >-
  Example code for the auth account REST method. Complete guide on how to use
  auth account REST method in GetBlock Web3 documentation.
---

# auth account

Returns an account's details, including its account number and sequence (nonce), as a protobuf-Any wrapped account type.

## Endpoint

```http
GET /cosmos/auth/v1beta1/accounts/{address}
```

## Path Parameters

| Parameter | Type   | Description                         |
| --------- | ------ | ----------------------------------- |
| address   | string | Bech32 account address (cosmos1...) |

## Example

{% code overflow="wrap" %}
```bash
export STABLE_REST=https://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/

curl "${STABLE_REST}cosmos/auth/v1beta1/accounts/stable1qg5ega6dykkxc307y25pecuufrjkxkagq9k7m4h"
```
{% endcode %}

## Response

```json
{
    "account": {
        "@type": "/cosmos.auth.v1beta1.BaseAccount",
        "address": "stable1qg5ega6dykkxc307y25pecuufrjkxkagq9k7m4h",
        "pub_key": {
            "@type": "/cosmos.crypto.secp256k1.PubKey",
            "key": "A1b2..."
        },
        "account_number": "123456",
        "sequence": "42"
    }
}
```

## Response Fields

| Field                   | Type   | Description                                       |
| ----------------------- | ------ | ------------------------------------------------- |
| account.address         | string | The account's bech32 address                      |
| account.account\_number | string | On-chain account number                           |
| account.sequence        | string | Account sequence (nonce) for the next transaction |

## Use Cases

* **Nonce management**: Read sequence before signing a transaction
* **Account checks**: Confirm an account exists on-chain

## Error Handling

| Error                     | Message       | Description                                                  |
| ------------------------- | ------------- | ------------------------------------------------------------ |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid                 |
| 404 / Not found           | Not found     | The account, validator, proposal, or resource does not exist |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect            |
