---
description: >-
  Example code for the auth account REST method. Complete guide on how to use
  the auth account REST method in GetBlock Web3 documentation.
---

# auth account - Cosmos

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
export COSMOS_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${COSMOS_REST}cosmos/auth/v1beta1/accounts/cosmos1xv9tklw7d82sezh9haa573wufgy59vmwe6xxe5"
```
{% endcode %}

## Response

```json
{
    "account": {
        "@type": "/cosmos.auth.v1beta1.BaseAccount",
        "address": "cosmos1xv9tklw7d82sezh9haa573wufgy59vmwe6xxe5",
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
