# estimateGas - Ethereum Classic

Returns the estimated gas a call or transaction would consume at a chosen block, the GraphQL equivalent of `eth_estimateGas`. Accessed within a block selection.

## Schema

```graphql
block { estimateGas(data: CallData!): Long! }
```

## Arguments

| Argument   | Type    | Description             |
| ---------- | ------- | ----------------------- |
| data.to    | Address | Target address          |
| data.data  | Bytes   | ABI-encoded call data   |
| data.from  | Address | Optional caller address |
| data.value | BigInt  | Optional value in wei   |

## Query

```graphql
query {
  block {
    estimateGas(data: {to: "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045", value: "1000000000000000000"})
  }
}
```

## Example

{% code overflow="wrap" %}
```bash
export ETC_GRAPHQL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl --location --request POST "${ETC_GRAPHQL}graphql" \
--header 'Content-Type: application/json' \
--data-raw '{"query": "query { block { estimateGas(data: {to: \"0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045\", value: \"1000000000000000000\"}) } }"}'
```
{% endcode %}

## Response

```json
{
    "data": {
        "block": {
            "estimateGas": 21000
        }
    }
}
```

## Response Fields

| Field       | Type | Description                      |
| ----------- | ---- | -------------------------------- |
| estimateGas | Long | Estimated gas units for the call |

## Use Cases

* **Gas Sizing**: Set a gas limit before sending
* **Fee Preview**: Multiply by gasPrice for a fee estimate
* **Validation**: Detect calls that would fail

## Error Handling

| Error                     | Message               | Description                                       |
| ------------------------- | --------------------- | ------------------------------------------------- |
| errors\[]                 | gas estimation failed | The call reverted during estimation               |
| 403 / RBAC: access denied | Access denied         | The GetBlock access token is missing or incorrect |
