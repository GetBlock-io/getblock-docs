---
description: >-
  Example code for the query(view_state) json-rpc method. Сomplete guide on how
  to use query(view_state) json-rpc in GetBlock.io Web3 documentation.
---

# query(view\_state) - NEAR Protocol

#### Parameters

`request_type` - string

type of request

`finality` - string

You should pick either that or block\_id.

Can be either "optimistic" or "final"

`block_id` - int or string

You should pick either that or finality.

The block\_id param can take either the block number OR the block hash as an argument.

`account_id` - string

account id

`prefix_base64` - string

prefix

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "query",
"params": {"request_type": "view_state", "finality": "final", "account_id": "staked.poolv1.near""prefix_base64": ""},
"id": "getblock.io"}'
```

#### Response

```java
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "values": [
            {
                "key": "bTo6MA==",
                "value": "eyJwcmVtaXVtIjp0cnVlLCJzZW5kZXIiOiJqb3NoZm9yZC50ZXN0bmV0IiwidGV4dCI6ImhlbGxvIn0=",
                "proof": []
            },
            {
                "key": "bTo6MQ==",
                "value": "eyJwcmVtaXVtIjpmYWxzZSwic2VuZGVyIjoiY2hhZG9oIiwidGV4dCI6ImhlbGxvIGVyeWJvZHkifQ==",
                "proof": []
            },
            {
                "key": "bTo6MTA=",
                "value": "eyJwcmVtaXVtIjpmYWxzZSwic2VuZGVyIjoic2F0b3NoaWYudGVzdG5ldCIsInRleHQiOiJIaWxsbyEifQ==",
                "proof": []
            },
            {
                "key": "bTo6MTE=",
                "value": "eyJwcmVtaXVtIjpmYWxzZSwic2VuZGVyIjoidmFsZW50aW5lc29rb2wudGVzdG5ldCIsInRleHQiOiJIaSEifQ==",
                "proof": []
            },
            {
                "key": "bTo6MTI=",
                "value": "eyJwcmVtaXVtIjp0cnVlLCJzZW5kZXIiOiJobngudGVzdG5ldCIsInRleHQiOiJoZWxsbyJ9",
                "proof": []
            },
            {
                "key": "bTo6MTM=",
                "value": "eyJwcmVtaXVtIjp0cnVlLCJzZW5kZXIiOiJobngudGVzdG5ldCIsInRleHQiOiJzZCJ9",
                "proof": []
            },
            {
                "key": "bTo6MTQ=",
                "value": "eyJwcmVtaXVtIjpmYWxzZSwic2VuZGVyIjoiamdoZy50ZXN0bmV0IiwidGV4dCI6IktoZyJ9",
                "proof": []
            },
            {
                "key": "bTo6MTU=",
                "value": "eyJwcmVtaXVtIjpmYWxzZSwic2VuZGVyIjoiYWNjb3VudC50ZXN0bmV0IiwidGV4dCI6IldoZW4gSUNPPyJ9",
                "proof": []
            },
            {
                "key": "bTo6MTY=",
                "value": "eyJwcmVtaXVtIjpmYWxzZSwic2VuZGVyIjoiYWNjb3VudC50ZXN0bmV0IiwidGV4dCI6IlRlc3QgMiJ9",
                "proof": []
            }
        ],
        "proof": [],
        "block_height": 17814234,
        "block_hash": "GT1D8nweVQU1zyCUv399x8vDv2ogVq71w17MyR66hXBB"
    }
}
```
