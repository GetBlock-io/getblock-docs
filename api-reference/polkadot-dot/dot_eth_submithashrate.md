---
description: >-
  Example code for the eth_submitHashrate  {disallowed} json-rpc method.
  Сomplete guide on how to use eth_submitHashrate  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# eth\_submitHashrate {disallowed} - Polkadot

#### Parameters

`index` - U256

None

`hash` - H256

None

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "eth_submitHashrate",
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
