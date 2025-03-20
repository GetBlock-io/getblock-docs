---
description: >-
  Example code for the /v1 json-rpc method. Сomplete guide on how to use /v1
  json-rpc in GetBlock.io Web3 documentation.
---

# /v1 - Aptos

#### Parameters

\-

#### Request

```java
curl --location --request GET 'https://go.getblock.io/<ACCESS-TOKEN>/v1/info' \
--header 'Content-Type: application/json'
```

#### Response

```java
{
    "block_height": "58851650",
    "chain_id": 1,
    "epoch": "2796",
    "git_hash": "6568c5ee6a58b4f96c0780d4f66d7e573e61c418",
    "ledger_timestamp": "1685696086534090",
    "ledger_version": "152087593",
    "node_role": "full_node",
    "oldest_block_height": "1101178",
    "oldest_ledger_version": "2287593"
}
```
