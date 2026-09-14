---
description: >-
  Example code for the /v2/accounts/{address}/assets REST method. Complete guide
  on how to use the /v2/accounts/{address}/assets REST method in the GetBlock
  Web3 documentation.
---

# /v2/accounts/{address}/assets - Algorand

Returns the Algorand Standard Assets (ASAs) held by an account, each with its amount and frozen status. Paginated.

## Endpoint

```http
GET /v2/accounts/{address}/assets
```

## Path Parameters

| Parameter | Type   | Description     |
| --------- | ------ | --------------- |
| address   | string | Account address |

## Query Parameters

| Parameter | Type    | Description              |
| --------- | ------- | ------------------------ |
| limit     | integer | Max results per page     |
| next      | string  | Pagination token         |
| asset-id  | integer | Filter to a single asset |

## Example

{% code overflow="wrap" %}
```bash
export ALGO_INDEXER=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_INDEXER}v2/accounts/XBYLS2E6YI6XXL5BWCAMOA4GTWHXWENZMX5UHXMRNWWUQ7BXCY5WC5TEPA/assets"
```
{% endcode %}

## Response

```json
{
    "assets": [
        {
            "asset-id": 31566704,
            "amount": 1000000,
            "is-frozen": false
        }
    ],
    "current-round": 35000000
}
```

## Response Fields

| Field         | Type    | Description                                           |
| ------------- | ------- | ----------------------------------------------------- |
| assets        | array   | ASA holdings with asset-id, amount, and frozen status |
| current-round | integer | Round the Indexer answered at                         |

## Use Cases

* **Portfolio**: List an account's token holdings
* **Wallet Backends**: Enumerate opted-in assets
* **Analytics**: Study holdings distribution

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 404 / Not found           | Not found     | No account matches the address                    |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
