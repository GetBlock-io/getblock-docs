---
description: >-
  Example code for the /v2/assets/{asset-id}/transactions REST method. Complete
  guide on how to use the /v2/assets/{asset-id}/transactions REST method in the
  GetBlock Web3 documentation.
---

# /v2/assets/{asset-id}/transactions - Algorand

Returns transactions involving a given ASA, with the same rich filters as the main transaction search. Paginated.

## Endpoint

```http
GET /v2/assets/{asset-id}/transactions
```

## Path Parameters

| Parameter | Type    | Description |
| --------- | ------- | ----------- |
| asset-id  | integer | Asset id    |

## Query Parameters

| Parameter | Type    | Description                |
| --------- | ------- | -------------------------- |
| address   | string  | Filter by involved address |
| limit     | integer | Max results                |
| min-round | integer | Lower round bound          |
| next      | string  | Pagination token           |

## Example

{% code overflow="wrap" %}
```bash
export ALGO_INDEXER=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_INDEXER}v2/assets/31566704/transactions"
```
{% endcode %}

## Response

```json
{
    "transactions": [
        {
            "id": "MEFEHTKVA6LP4CIFC3VNJNXFXPBPHQMBNK5CGWQENTNJXCTS3ELA",
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

| Field        | Type   | Description                      |
| ------------ | ------ | -------------------------------- |
| transactions | array  | Transactions involving the asset |
| next-token   | string | Pagination token                 |

## Use Cases

* **Token Activity**: Track transfers of an ASA
* **Volume Analysis**: Aggregate asset transaction volume
* **Compliance**: Trace asset movements

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 404 / Not found           | Not found     | No asset matches the id                           |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
