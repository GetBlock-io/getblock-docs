# account - Ethereum Classic

Returns an account's state — balance, transaction count (nonce), contract code, and storage — at a chosen block. Accessed within a block selection so the state is read at a specific height.

## Schema

```graphql
block { account(address: Address!): Account! }
```

## Arguments

| Argument     | Type     | Description                                      |
| ------------ | -------- | ------------------------------------------------ |
| address      | Address! | 20-byte account address                          |
| block.number | Long     | Block context for the read (via the block field) |

## Query

```graphql
query {
  block(number: 21200000) {
    account(address: "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045") {
      balance
      transactionCount
      code
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
--data-raw '{"query": "query { block(number: 21200000) { account(address: \"0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045\") { balance transactionCount code } } }"}'
```
{% endcode %}

## Response

```json
{
    "data": {
        "block": {
            "account": {
                "balance": "500000000000000000000",
                "transactionCount": 42,
                "code": "0x"
            }
        }
    }
}
```

## Response Fields

| Field            | Type   | Description                         |
| ---------------- | ------ | ----------------------------------- |
| balance          | BigInt | Account balance in wei at the block |
| transactionCount | Long   | Number of transactions sent (nonce) |
| code             | Bytes  | Contract bytecode, or 0x for an EOA |

## Use Cases

* **Balance Reads**: Read balance at a specific block
* **Contract Detection**: Check code to distinguish EOAs from contracts
* **Nonce**: Read transactionCount before signing

## Error Handling

| Error                     | Message         | Description                                       |
| ------------------------- | --------------- | ------------------------------------------------- |
| errors\[]                 | invalid address | The address is malformed                          |
| 403 / RBAC: access denied | Access denied   | The GetBlock access token is missing or incorrect |
