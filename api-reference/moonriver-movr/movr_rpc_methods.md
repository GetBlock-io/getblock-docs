---
description: >-
  Example code for the rpc_methods json-rpc method. Сomplete guide on how to use
  rpc_methods json-rpc in GetBlock.io Web3 documentation.
---

# rpc\_methods - Moonriver

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "rpc_methods",
"params": [],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "methods": [
            "chain_getBlock",
            "chain_getBlockHash",
            "chain_getFinalisedHead",
            "chain_getFinalizedHead",
            "chain_getHead",
            "chain_getHeader",
            "chain_getRuntimeVersion",
            "chain_subscribeAllHeads",
            "chain_subscribeFinalisedHeads",
            "chain_subscribeFinalizedHeads"
        ]
    }
}
```
