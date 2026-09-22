# bank balances

Returns all coin balances held by an address, each as a denom and amount. Native AKT is reported in uakt.

## Endpoint

```http
GET /cosmos/bank/v1beta1/balances/{address}
```

## Path Parameters

| Parameter | Type   | Description                      |
| --------- | ------ | -------------------------------- |
| address   | string | Bech32 account address (akash1…) |

## Query Parameters

| Parameter        | Type   | Description |
| ---------------- | ------ | ----------- |
| pagination.limit | string | Max results |

## Example

{% code overflow="wrap" %}
```bash
export AKASH_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${AKASH_REST}cosmos/bank/v1beta1/balances/akash1nl7dg3xj9j2y6q7z0v4w8c2b5d1f3g6h9k2m4p"
```
{% endcode %}

## Response

```json
{
    "balances": [
        {
            "denom": "uakt",
            "amount": "150000000"
        }
    ],
    "pagination": {
        "total": "1"
    }
}
```

## Response Fields

| Field            | Type   | Description                      |
| ---------------- | ------ | -------------------------------- |
| balances         | array  | Coin balances as {denom, amount} |
| pagination.total | string | Total denoms                     |

## Use Cases

* **Balance Reads**: Display AKT and token balances
* **Portfolio**: Enumerate holdings
* **Accounting**: Reconcile balances

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A parameter was invalid                           |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
