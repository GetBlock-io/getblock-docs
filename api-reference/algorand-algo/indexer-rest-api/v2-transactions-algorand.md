---
description: >-
  Example code for the /v2/transactions REST method. Complete guide on how to
  use the /v2/transactions REST method in the GetBlock Web3 documentation.
---

# /v2/transactions - Algorand

Searches all transactions with a rich set of filters — address, asset id, application id, round and time ranges, transaction type, amount, note prefix, and more. The primary Indexer search endpoint.

## Endpoint

```http
GET /v2/transactions
```

## Query Parameters

| Parameter      | Type    | Description                          |
| -------------- | ------- | ------------------------------------ |
| address        | string  | Filter by an involved address        |
| asset-id       | integer | Filter by asset                      |
| application-id | integer | Filter by application                |
| tx-type        | string  | pay, axfer, appl, acfg, afrz, keyreg |
| min-round      | integer | Lower round bound                    |
| limit          | integer | Max results                          |

## Example

{% code overflow="wrap" %}
```bash
export ALGO_INDEXER=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_INDEXER}v2/transactions"
```
{% endcode %}

## Response

```json
{
    "transactions": [
        {
            "id": "MEFEHTKVA6LP4CIFC3VNJNXFXPBPHQMBNK5CGWQENTNJXCTS3ELA",
            "sender": "XBYLS2E6YI6XXL5BWCAMOA4GTWHXWENZMX5UHXMRNWWUQ7BXCY5WC5TEPA",
            "tx-type": "axfer",
            "confirmed-round": 35000000,
            "asset-transfer-transaction": {
                "asset-id": 31566704,
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

| Field        | Type   | Description           |
| ------------ | ------ | --------------------- |
| transactions | array  | Matching transactions |
| next-token   | string | Pagination token      |

## Use Cases

* **Indexing**: Backfill transactions by filter
* **Compliance**: Search by address or asset
* **Analytics**: Query activity by application or type

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A filter parameter is invalid                     |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
