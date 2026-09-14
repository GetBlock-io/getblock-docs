---
description: >-
  Example code for the /v2/accounts/{address} REST method. Complete guide on how
  to use the /v2/accounts/{address} REST method in the GetBlock Web3
  documentation.
---

# /v2/accounts/{address} - Algorand

Returns the current state of an account: its ALGO balance, minimum balance, participation status, held assets, and created/opted-in applications. Balances are in microAlgos.

## Endpoint

```http
GET /v2/accounts/{address}
```

## Path Parameters

| Parameter | Type   | Description                      |
| --------- | ------ | -------------------------------- |
| address   | string | Account address (58-char base32) |

## Query Parameters

| Parameter | Type   | Description                                                 |
| --------- | ------ | ----------------------------------------------------------- |
| exclude   | string | Set to 'all' to omit assets and apps for a lighter response |

## Example

{% code overflow="wrap" %}
```bash
export ALGO_ALGOD=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_ALGOD}v2/accounts/XBYLS2E6YI6XXL5BWCAMOA4GTWHXWENZMX5UHXMRNWWUQ7BXCY5WC5TEPA"
```
{% endcode %}

## Response

```json
{
    "address": "XBYLS2E6YI6XXL5BWCAMOA4GTWHXWENZMX5UHXMRNWWUQ7BXCY5WC5TEPA",
    "amount": 5000000,
    "amount-without-pending-rewards": 5000000,
    "min-balance": 100000,
    "round": 35000000,
    "status": "Offline",
    "assets": [
        {
            "asset-id": 31566704,
            "amount": 1000000,
            "is-frozen": false
        }
    ],
    "total-apps-opted-in": 2,
    "total-assets-opted-in": 1
}
```

## Response Fields

| Field       | Type    | Description                           |
| ----------- | ------- | ------------------------------------- |
| amount      | integer | Account balance in microAlgos         |
| min-balance | integer | Minimum balance required (microAlgos) |
| assets      | array   | ASA holdings with asset-id and amount |
| status      | string  | Participation status (Online/Offline) |

## Use Cases

* **Balance Reads**: Display an account's ALGO and asset balances
* **Min-Balance Checks**: Verify an account can cover its minimum balance
* **Wallet Backends**: Read holdings and opt-ins

## Error Handling

| Error                     | Message         | Description                                       |
| ------------------------- | --------------- | ------------------------------------------------- |
| 400 / Invalid address     | Invalid address | The address is not valid base32                   |
| 403 / RBAC: access denied | Access denied   | The GetBlock access token is missing or incorrect |
