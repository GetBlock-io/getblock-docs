# call - Ethereum Classic

Executes a read-only message call against contract state at a chosen block and returns the output data, gas used, and status — the GraphQL equivalent of `eth_call`. Accessed within a block selection.

## Schema

```graphql
block { call(data: CallData!): CallResult }
```

## Arguments

| Argument  | Type    | Description             |
| --------- | ------- | ----------------------- |
| data.to   | Address | Target contract address |
| data.data | Bytes   | ABI-encoded call data   |
| data.from | Address | Optional caller address |

## Query

```graphql
query {
  block {
    call(data: {to: "0x82A618305706B14e7bcf2592D4B9324A366b6dAd", data: "0x06fdde03"}) {
      data
      status
      gasUsed
    }
  }
}
```

## Example

{% code overflow="wrap" %}
```bash
export ETC_GRAPHQL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl --location --request POST "${ETC_GRAPHQL}graphql" \
--header 'Content-Type: application/json' \
--data-raw '{"query": "query { block { call(data: {to: \"0x82A618305706B14e7bcf2592D4B9324A366b6dAd\", data: \"0x06fdde03\"}) { data status gasUsed } } }"}'
```
{% endcode %}

## Response

```json
{
    "data": {
        "block": {
            "call": {
                "data": "0x000000...",
                "status": 1,
                "gasUsed": 24000
            }
        }
    }
}
```

## Response Fields

| Field   | Type  | Description               |
| ------- | ----- | ------------------------- |
| data    | Bytes | ABI-encoded return value  |
| status  | Long  | 1 on success, 0 on revert |
| gasUsed | Long  | Gas consumed by the call  |

## Use Cases

* **Contract Reads**: Call a view function without a transaction
* **Token Balances**: Read `balanceOf` and other getters
* **Dry Runs**: Check a call would succeed before sending

## Error Handling

| Error                     | Message            | Description                                       |
| ------------------------- | ------------------ | ------------------------------------------------- |
| errors\[]                 | execution reverted | The call reverted; status is 0                    |
| 403 / RBAC: access denied | Access denied      | The GetBlock access token is missing or incorrect |
