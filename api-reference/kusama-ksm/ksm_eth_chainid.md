---
description: >-
  Example code for the eth_chainId  {disallowed} json-rpc method. Сomplete guide
  on how to use eth_chainId  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# eth\_chainId {disallowed} - Kusama

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "eth_chainId",
"params": [],
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
