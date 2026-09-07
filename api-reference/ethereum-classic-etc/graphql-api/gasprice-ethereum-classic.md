# gasPrice - Ethereum Classic

Returns the current gas price in wei. Ethereum Classic uses legacy gas pricing, so this is the price to set on a transaction.

## Schema

```graphql
gasPrice: BigInt!
```

## Query

```graphql
query {
  gasPrice
}
```

## Example

{% code overflow="wrap" %}
```bash
export ETC_GRAPHQL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl --location --request POST "${ETC_GRAPHQL}graphql" \
--header 'Content-Type: application/json' \
--data-raw '{"query": "query { gasPrice }"}'
```
{% endcode %}

## Response

```json
{
    "data": {
        "gasPrice": "1000000000"
    }
}
```

## Response Fields

| Field    | Type   | Description              |
| -------- | ------ | ------------------------ |
| gasPrice | BigInt | Current gas price in wei |

## Use Cases

* **Fee Estimation**: Set gasPrice on a legacy transaction
* **Cost Monitoring**: Track gas price over time
* **Wallets**: Populate the fee field

## Error Handling

| Error                     | Message        | Description                                       |
| ------------------------- | -------------- | ------------------------------------------------- |
| errors\[]                 | internal error | The node failed to return the gas price           |
| 403 / RBAC: access denied | Access denied  | The GetBlock access token is missing or incorrect |
