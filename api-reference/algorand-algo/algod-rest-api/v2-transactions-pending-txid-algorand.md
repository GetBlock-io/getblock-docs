---
description: >-
  Example code for the /v2/transactions/pending/{txid} REST method. Complete
  guide on how to use the /v2/transactions/pending/{txid} REST method in the
  GetBlock Web3 documentation.
---

# /v2/transactions/pending/{txid} - Algorand

Returns information about a transaction in the pool by its id, including the round it was confirmed in once it is committed. Poll this after submitting to confirm inclusion.

## Endpoint

```http
GET /v2/transactions/pending/{txid}
```

## Path Parameters

| Parameter | Type   | Description    |
| --------- | ------ | -------------- |
| txid      | string | Transaction id |

## Example

{% code overflow="wrap" %}
```bash
export ALGO_ALGOD=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_ALGOD}v2/transactions/pending/MEFEHTKVA6LP4CIFC3VNJNXFXPBPHQMBNK5CGWQENTNJXCTS3ELA"
```
{% endcode %}

## Response

```json
{
    "confirmed-round": 35000000,
    "pool-error": "",
    "txn": {
        "txn": {
            "snd": "XBYLS2E6YI6XXL5BWCAMOA4GTWHXWENZMX5UHXMRNWWUQ7BXCY5WC5TEPA",
            "type": "pay",
            "amt": 1000000
        }
    }
}
```

## Response Fields

| Field           | Type    | Description                                                 |
| --------------- | ------- | ----------------------------------------------------------- |
| confirmed-round | integer | Round the transaction was confirmed in (0 if still pending) |
| pool-error      | string  | Non-empty if the transaction was rejected                   |
| txn             | object  | The transaction                                             |

## Use Cases

* **Confirmation Polling**: Wait for confirmed-round after submitting
* **Failure Detection**: Read pool-error for rejections
* **Receipts**: Confirm a payment landed

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 404 / Not found           | Not found     | The transaction is not in the pool                |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
