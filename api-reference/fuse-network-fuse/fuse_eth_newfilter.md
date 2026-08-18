---
description: >-
  Example code for the eth_newFilter json-rpc method. Сomplete guide on how to
  use eth_newFilter json-rpc in GetBlock.io Web3 documentation.
---

# eth\_newFilter - Fuse Network

#### Parameters

`Object` - object

Filter options

#### Request

```java
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "eth_newFilter",
"params": [{"fromBlock": "earliest", "toBlock": "latest", "topics": []}],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "0x175"
}
```
