---
description: >-
  Example code for the eth_getBlockByHash  {disallowed} json-rpc method.
  Сomplete guide on how to use eth_getBlockByHash  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# eth\_getBlockByHash {disallowed} - Kusama

#### Parameters

`hash` - H256

None

`full` - bool

None

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getBlockByHash",
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
