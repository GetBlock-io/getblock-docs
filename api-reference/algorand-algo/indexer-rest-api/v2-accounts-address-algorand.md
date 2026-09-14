---
description: >-
  Example code for the /v2/accounts/{address} REST method. Complete guide on how
  to use the /v2/accounts/{address} REST method in the GetBlock Web3
  documentation.
---

# /v2/accounts/{address} - Algorand

Returns an account's state from the Indexer, optionally as of a specific round.

{% hint style="info" %}
Unlike algod, the Indexer can return historical account state and includes the round the data applies to.
{% endhint %}

## Endpoint

```http
GET /v2/accounts/{address}
```

## Path Parameters

| Parameter | Type   | Description     |
| --------- | ------ | --------------- |
| address   | string | Account address |

## Query Parameters

| Parameter   | Type    | Description                          |
| ----------- | ------- | ------------------------------------ |
| round       | integer | Include results for a specific round |
| include-all | boolean | Include deleted assets/apps          |

## Example

{% code overflow="wrap" %}
```bash
export ALGO_INDEXER=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_INDEXER}v2/accounts/XBYLS2E6YI6XXL5BWCAMOA4GTWHXWENZMX5UHXMRNWWUQ7BXCY5WC5TEPA"
```
{% endcode %}

## Response

```json
{
    "account": {
        "address": "XBYLS2E6YI6XXL5BWCAMOA4GTWHXWENZMX5UHXMRNWWUQ7BXCY5WC5TEPA",
        "amount": 5000000,
        "min-balance": 100000,
        "assets": [
            {
                "asset-id": 31566704,
                "amount": 1000000
            }
        ],
        "created-at-round": 20000000
    },
    "current-round": 35000000
}
```

## Response Fields

| Field                    | Type    | Description                      |
| ------------------------ | ------- | -------------------------------- |
| account.amount           | integer | Balance in microAlgos            |
| account.created-at-round | integer | Round the account was first seen |
| current-round            | integer | Round the Indexer answered at    |

## Use Cases

* **Historical Balances**: Read account state at a past round
* **Explorers**: Render account pages with history
* **Analytics**: Study account state over time

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 404 / Not found           | Not found     | No account matches the address                    |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
