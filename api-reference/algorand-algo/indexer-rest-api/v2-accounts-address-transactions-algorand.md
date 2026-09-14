---
description: >-
  Example code for the /v2/accounts/{address}/transactions REST method. Complete
  guide on how to use the /v2/accounts/{address}/transactions REST method in the
  GetBlock Web3 documentation.
---

# /v2/accounts/{address}/transactions - Algorand

Returns the transaction history for an account, with rich filters: by round range, time range, asset ID, transaction type, amount, and more. Paginated with a next-token.

## Endpoint

```http
GET /v2/accounts/{address}/transactions
```

## Path Parameters

| Parameter | Type   | Description     |
| --------- | ------ | --------------- |
| address   | string | Account address |

## Query Parameters

| Parameter | Type    | Description            |
| --------- | ------- | ---------------------- |
| limit     | integer | Max results per page   |
| next      | string  | Pagination token       |
| min-round | integer | Lower round bound      |
| max-round | integer | Upper round bound      |
| asset-id  | integer | Filter by asset        |
| tx-type   | string  | pay, axfer, appl, etc. |

## Example

{% code overflow="wrap" %}
```bash
export ALGO_INDEXER=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_INDEXER}v2/accounts/XBYLS2E6YI6XXL5BWCAMOA4GTWHXWENZMX5UHXMRNWWUQ7BXCY5WC5TEPA/transactions"
```
{% endcode %}

## Response

```json
{
    "transactions": [
        {
            "id": "MEFEHTKVA6LP4CIFC3VNJNXFXPBPHQMBNK5CGWQENTNJXCTS3ELA",
            "sender": "XBYLS2E6YI6XXL5BWCAMOA4GTWHXWENZMX5UHXMRNWWUQ7BXCY5WC5TEPA",
            "tx-type": "pay",
            "confirmed-round": 35000000,
            "payment-transaction": {
                "amount": 1000000,
                "receiver": "XBYLS2E6YI6XXL5BWCAMOA4GTWHXWENZMX5UHXMRNWWUQ7BXCY5WC5TEPA"
            }
        }
    ],
    "current-round": 35000000,
    "next-token": "b..."
}
```

## Response Fields

| Field         | Type    | Description                   |
| ------------- | ------- | ----------------------------- |
| transactions  | array   | Matching transactions         |
| next-token    | string  | Token for the next page       |
| current-round | integer | Round the Indexer answered at |

## Use Cases

* **Wallet History**: List an account's transactions
* **Accounting**: Filter by asset or type for reconciliation
* **Explorers**: Paginate account activity

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A filter parameter is invalid                     |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
