---
description: >-
  Example code for the getrawtransaction json-rpc method. Сomplete guide on how
  to use getrawtransaction json-rpc in GetBlock.io Web3 documentation.
---

# getrawtransaction - DigiByte

#### Parameters

`txid` - string, required

The transaction id

`verbose` - boolean, optional, default=false

If false, return a string, otherwise return a json object

`blockhash` - string, optional

The block in which to look for the transaction

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "getrawtransaction",
"params": ["4f0934ca5affc6069dc2a112357ef7a2afcdc40307f81ebeccd86e23fddfa2db", false, null],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": "010000000001010000000000000000000000000000000000000000000000000000000000000000ffffffff580426efcf00042d133b610cfabe6d6d4b6e5eda06373bc46000cf601b661f9243db16f482156a91d7c9008ca9a1d4c501000000000000000a2a6172e08a7c0100000a6b14363931303037343761363664363235343236363600000000020000000000000000266a24aa21a9ed47542e50fcec7a7b535c2b6a35cf4ac18722ad3289d9b90d037124862db8ec3e6c9770930b0000001976a9146f5cf1be10f3ad794eca67821fb280305b2900a188ac0120000000000000000000000000000000000000000000000000000000000000000000000000"
}
```
