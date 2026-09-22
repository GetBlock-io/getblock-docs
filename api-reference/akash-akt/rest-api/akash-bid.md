# akash bid

Returns a single provider bid by its full id.

## Endpoint

```
GET /akash/market/v1beta4/bids/info
```

## Query Parameters

| Parameter   | Type   | Description         |
| ----------- | ------ | ------------------- |
| id.owner    | string | Owner               |
| id.dseq     | string | Deployment sequence |
| id.gseq     | string | Group sequence      |
| id.oseq     | string | Order sequence      |
| id.provider | string | Provider address    |

## Example

{% code overflow="wrap" %}
```bash
export AKASH_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${AKASH_REST}akash/market/v1beta4/bids/info?id.owner=akash1nl7dg3xj9j2y6q7z0v4w8c2b5d1f3g6h9k2m4p&id.dseq=12345678&id.gseq=1&id.oseq=1&id.provider=akashvaloper1nl7dg3xj9j2y6q7z0v4w8c2b5d1f3g6h9k2m4p"
```
{% endcode %}

## Response

```json
{
    "bid": {
        "bid_id": {
            "owner": "akash1nl7dg3xj9j2y6q7z0v4w8c2b5d1f3g6h9k2m4p",
            "dseq": "12345678",
            "provider": "akashvaloper1nl7dg3xj9j2y6q7z0v4w8c2b5d1f3g6h9k2m4p"
        },
        "state": "open",
        "price": {
            "denom": "uakt",
            "amount": "1.5"
        }
    },
    "escrow_account": {
        "balance": {
            "denom": "uakt",
            "amount": "5000000"
        }
    }
}
```

## Response Fields

| Field           | Type   | Description              |
| --------------- | ------ | ------------------------ |
| bid             | object | Bid id, state, and price |
| escrow\_account | object | Bid escrow balance       |

## Use Cases

* **Price Discovery**: Inspect a specific bid

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A parameter was invalid                           |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
