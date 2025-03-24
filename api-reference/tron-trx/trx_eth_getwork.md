---
description: >-
  Example code for the eth_getWork json-rpc method. Сomplete guide on how to use
  eth_getWork json-rpc in GetBlock.io Web3 documentation.
---

# eth\_getWork - TRON

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getWork",
"params": [],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": [
        "0x00000000025ad7ec652741485c5410bcfb36ae1047d9ef617b406cec8f87f3c6",
        null,
        null
    ]
}
```
