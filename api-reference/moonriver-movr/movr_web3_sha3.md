---
description: >-
  Example code for the web3_sha3  {disallowed} json-rpc method. Сomplete guide
  on how to use web3_sha3  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# web3\_sha3 {disallowed} - Moonriver

#### Parameters

`data` - Bytes

None

#### Request

```java
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "web3_sha3",
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
