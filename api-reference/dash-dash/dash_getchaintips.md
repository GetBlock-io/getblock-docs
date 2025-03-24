---
description: >-
  Example code for the getchaintips json-rpc method. Сomplete guide on how to
  use getchaintips json-rpc in GetBlock.io Web3 documentation.
---

# getchaintips - Dash

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "getchaintips",
"params": [],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": [
        {
            "branchlen": 0,
            "chainwork": "00000000000000000000000000000000000000000000615f0d845c321319b10d",
            "difficulty": 103704145.924301,
            "forkpoint": "0000000000000019cd3a50eedc418372f7aa9060d6ded56a769a51a93dfe0e4a",
            "hash": "0000000000000019cd3a50eedc418372f7aa9060d6ded56a769a51a93dfe0e4a",
            "height": 1535914,
            "status": "active"
        },
        {
            "branchlen": 1,
            "chainwork": "00000000000000000000000000000000000000000000610234e38aca75891c6a",
            "difficulty": 148138019.9348546,
            "forkpoint": "000000000000001246d5a2ed6cf8e136a1b2c97ca71bf924ee829ca02ef90176",
            "hash": "0000000000000007c48c36c74265280071ca05555e53f09613cf46a22bc531df",
            "height": 1533030,
            "status": "valid-headers"
        }
    ]
}
```
