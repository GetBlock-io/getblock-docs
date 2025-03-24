---
description: >-
  Example code for the on_get_block_hash json-rpc method. Сomplete guide on how
  to use on_get_block_hash json-rpc in GetBlock.io Web3 documentation.
---

# on\_get\_block\_hash - Monero

#### Parameters

`block height` - int array of length 1

block height

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "on_get_block_hash",
"params": {"block height": 912345},
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "e22cf75f39ae720e8b71b3d120a5ac03f0db50bba6379e2850975b4859190bc6"
}
```
