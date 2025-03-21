---
description: >-
  Example code for the addnode  {disallowed} json-rpc method. Сomplete guide on
  how to use addnode  {disallowed} json-rpc in GetBlock.io Web3 documentation.
---

# addnode {disallowed} - Bitcoin

#### Parameters

`node` - string, required

The node (see getpeerinfo for nodes)

`command` - string, required

‘add’ to add a node to the list, ‘remove’ to remove a node from the list, ‘onetry’ to try a connection to the node once

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/v1/mainnet/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "addnode",
"params": [null, null],
"id": "getblock.io"}'
```

#### Response

```java
{
    "result": "null",
    "id": "getblock.io",
    "status_code": 405,
    "message": "Method not allowed"
}
```
