---
description: >-
  Example code for the auth account JSON-RPC method. Complete guide on how to
  use auth account JSON-RPC in GetBlock Web3 documentation.
---

# /cosmos/auth/v1beta1/accounts/{address} - TAC

Returns an account's details, including its account number and sequence (nonce), as a protobuf-Any wrapped account type.

## Endpoint

```
GET /cosmos/auth/v1beta1/accounts/{address}
```

## Path Parameters

| Parameter | Type   | Description                      |
| --------- | ------ | -------------------------------- |
| address   | string | Bech32 account address (tac1...) |

## Example

{% code overflow="wrap" %}
```bash
export TAC_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${TAC_REST}cosmos/auth/v1beta1/accounts/tac1ql8dru5dgm6gtg3hcdq9lds3q3fnk3s6k9h8f0"
```
{% endcode %}

## Response

```json
{
    "account": {
        "@type": "/cosmos.auth.v1beta1.BaseAccount",
        "address": "tac1ql8dru5dgm6gtg3hcdq9lds3q3fnk3s6k9h8f0",
        "pub_key": {
            "@type": "/cosmos.crypto.secp256k1.PubKey",
            "key": "A1b2..."
        },
        "account_number": "12345",
        "sequence": "8"
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
