---
description: >-
  Example code for the /v2/transactions/{txid} REST method. Complete guide on
  how to use the /v2/transactions/{txid} REST method in the GetBlock Web3
  documentation.
---

# /v2/transactions/{txid} - Algorand

Returns a single transaction by its id, with full details including its type-specific fields, confirmed round, fee, and any inner transactions or logs.

## Endpoint

```http
GET /v2/transactions/{txid}
```

## Path Parameters

| Parameter | Type   | Description    |
| --------- | ------ | -------------- |
| txid      | string | Transaction id |

## Example

{% code overflow="wrap" %}
```bash
export ALGO_INDEXER=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_INDEXER}v2/transactions/MEFEHTKVA6LP4CIFC3VNJNXFXPBPHQMBNK5CGWQENTNJXCTS3ELA"
```
{% endcode %}

## Response

```json
{
    "transaction": {
        "id": "MEFEHTKVA6LP4CIFC3VNJNXFXPBPHQMBNK5CGWQENTNJXCTS3ELA",
        "sender": "XBYLS2E6YI6XXL5BWCAMOA4GTWHXWENZMX5UHXMRNWWUQ7BXCY5WC5TEPA",
        "tx-type": "pay",
        "fee": 1000,
        "confirmed-round": 35000000,
        "round-time": 1730000000,
        "payment-transaction": {
            "amount": 1000000,
            "receiver": "XBYLS2E6YI6XXL5BWCAMOA4GTWHXWENZMX5UHXMRNWWUQ7BXCY5WC5TEPA"
        }
    },
    "current-round": 35000000
}
```

## Response Fields

| Field                       | Type    | Description                              |
| --------------------------- | ------- | ---------------------------------------- |
| transaction.tx-type         | string  | Transaction type (pay, axfer, appl, ...) |
| transaction.confirmed-round | integer | Round it was confirmed in                |
| transaction.fee             | integer | Fee paid in microAlgos                   |

## Use Cases

* **Transaction Lookup**: Fetch a transaction by id
* **Receipts**: Confirm details of a payment or app call
* **Explorers**: Render transaction pages

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 404 / Not found           | Not found     | No transaction matches the id                     |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
