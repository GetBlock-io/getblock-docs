---
description: >-
  Example code for the mmr_root  {disallowed} json-rpc method. Сomplete guide on
  how to use mmr_root  {disallowed} json-rpc in GetBlock.io Web3 documentation.
---

# mmr\_root {disallowed} - Kusama

#### Parameters

`at` - Hash

block hash

#### Request

```java
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "mmr_root",
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
