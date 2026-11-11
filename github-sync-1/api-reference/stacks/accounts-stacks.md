---
description: >-
  Example code for the accounts REST method. Complete guide on how to use
  accounts REST method in GetBlock Web3 documentation.
---

# accounts - Stacks

Returns an account's STX balance, locked (stacked) balance, unlock height, and next nonce. Balances are hex-encoded microSTX. A principal is a standard address or a contract principal (address.contract-name).

## Endpoint

```http
GET /v2/accounts/{principal}
```

## Path Parameters

| Parameter | Type   | Description                      |
| --------- | ------ | -------------------------------- |
| principal | string | A standard or contract principal |

## Query Parameters

| Parameter | Type   | Description                                               |
| --------- | ------ | --------------------------------------------------------- |
| proof     | number | Set to 0 to omit the MARF Merkle proofs from the response |

## Example

{% code overflow="wrap" %}
```bash
export STACKS_RPC=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${STACKS_RPC}v2/accounts/SP2J6ZY48GV1EZ5V2V5RB9MP66SW86PYKKNRV9EJ7?proof=0"
```
{% endcode %}

## Response

```json
{
    "balance": "0x00000000000000000000000005f5e100",
    "locked": "0x00000000000000000000000000000000",
    "unlock_height": 0,
    "nonce": 15,
    "balance_proof": "",
    "nonce_proof": ""
}
```

## Response Fields

| Field          | Type   | Description                                     |
| -------------- | ------ | ----------------------------------------------- |
| balance        | string | Total STX balance in microSTX (hex)             |
| locked         | string | Stacked (locked) balance in microSTX (hex)      |
| unlock\_height | number | Burn height at which locked STX unlock          |
| nonce          | number | The account's next transaction nonce            |
| balance\_proof | string | MARF proof for the balance (empty when proof=0) |
| nonce\_proof   | string | MARF proof for the nonce (empty when proof=0)   |

## Use Cases

* **Balance display**: Decode the hex microSTX balance (1 STX = 1,000,000 microSTX)
* **Nonce management**: Read the next nonce before building a transaction

## Error Handling

| Error                     | Message       | Description                                                                  |
| ------------------------- | ------------- | ---------------------------------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid or the transaction was rejected |
| 404 / Not found           | Not found     | The account, contract, map entry, variable, or constant does not exist       |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect                            |
