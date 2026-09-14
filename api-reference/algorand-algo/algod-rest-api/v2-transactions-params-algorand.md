---
description: >-
  Example code for the /v2/transactions/params REST method. Complete guide on
  how to use the /v2/transactions/params REST method in the GetBlock Web3
  documentation.
---

# /v2/transactions/params - Algorand

Returns the parameters needed to build a valid transaction: the suggested fee, the current first/last valid rounds, the genesis id and hash, and the minimum fee. Required to construct any transaction.

## Endpoint

```http
GET /v2/transactions/params
```

## Example

{% code overflow="wrap" %}
```bash
export ALGO_ALGOD=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_ALGOD}v2/transactions/params"
```
{% endcode %}

## Response

```json
{
    "consensus-version": "future",
    "fee": 0,
    "min-fee": 1000,
    "genesis-id": "mainnet-v1.0",
    "genesis-hash": "wGHE2Pwdvd7S12BL5FaOP20EGYesN73ktiC1qzkkit8=",
    "last-round": 35000000
}
```

## Response Fields

| Field        | Type    | Description                                   |
| ------------ | ------- | --------------------------------------------- |
| min-fee      | integer | Minimum fee in microAlgos (per transaction)   |
| genesis-hash | string  | Genesis hash to include in the transaction    |
| last-round   | integer | Current round; set first/last valid around it |

## Use Cases

* **Transaction Building**: Fetch params before constructing a transaction
* **Fee Setting**: Read min-fee to price a transaction
* **Validity Window**: Set first/last valid rounds

## Error Handling

| Error                     | Message        | Description                                       |
| ------------------------- | -------------- | ------------------------------------------------- |
| 500 / Internal            | Internal error | The node failed to return params                  |
| 403 / RBAC: access denied | Access denied  | The GetBlock access token is missing or incorrect |
