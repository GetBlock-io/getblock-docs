---
description: >-
  Example code for the getaddednodeinfo  {disallowed} json-rpc method. Сomplete
  guide on how to use getaddednodeinfo  {disallowed} json-rpc in GetBlock.io
  Web3 documentation.
---

# getaddednodeinfo {disallowed} - Dash

#### Parameters

`node` - string

Optional.

The node to get information about in the same IP\_address:port format as the addnode RPC. If this parameter is not provided, information about all added nodes will be returned

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "getaddednodeinfo",
"params": [null],
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
