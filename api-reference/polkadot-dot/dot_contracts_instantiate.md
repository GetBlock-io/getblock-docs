---
description: >-
  Example code for the contracts_instantiate  {disallowed} json-rpc method.
  Сomplete guide on how to use contracts_instantiate  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# contracts\_instantiate {disallowed} - Polkadot

#### Parameters

`request` - InstantiateRequest

None

`at` - BlockHash

None

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "contracts_instantiate",
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
