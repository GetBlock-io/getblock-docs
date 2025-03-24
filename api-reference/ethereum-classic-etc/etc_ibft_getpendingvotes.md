---
description: >-
  Example code for the ibft_getPendingVotes  {disallowed} json-rpc method.
  Сomplete guide on how to use ibft_getPendingVotes  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# ibft\_getPendingVotes {disallowed} - Ethereum Classic

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "ibft_getPendingVotes",
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
