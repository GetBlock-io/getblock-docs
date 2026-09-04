# /cosmos/bank/v1beta1/supply - Cronos

Returns the total supply of every token denom on the chain, paginated.

## Endpoint

```http
GET /cosmos/bank/v1beta1/supply
```

## Query Parameters

| Parameter        | Type   | Description           |
| ---------------- | ------ | --------------------- |
| pagination.limit | string | Max results to return |

## Example

{% code overflow="wrap" %}
```bash
export CRONOS_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${CRONOS_REST}cosmos/bank/v1beta1/supply"
```
{% endcode %}

## Response

```json
{
    "supply": [
        {
            "denom": "basecro",
            "amount": "30000000000000000000000000000"
        }
    ],
    "pagination": {
        "next_key": null,
        "total": "1"
    }
}
```

## Response Fields

| Field  | Type  | Description                               |
| ------ | ----- | ----------------------------------------- |
| supply | array | Total supply per denom as {denom, amount} |

## Use Cases

* **Tokenomics**: Read circulating base-denom supply
* **Analytics**: Track supply changes over time
* **Explorers**: Show total supply on a stats page

## Error Handling

| Error                     | Message        | Description                                       |
| ------------------------- | -------------- | ------------------------------------------------- |
| 500 / internal            | Internal error | The node failed to return supply                  |
| 403 / RBAC: access denied | Access denied  | The GetBlock access token is missing or incorrect |
