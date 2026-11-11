---
description: >-
  Example code for the grandpa_proveFinality  {disallowed} json-rpc method.
  Сomplete guide on how to use grandpa_proveFinality  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# grandpa\_proveFinality {disallowed} - Moonriver

#### Parameters

`begin` - BlockHash

start block hash

`end` - BlockHash

end block hash

`authoritiesSetId` - u64

set id

#### Request

```java
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "grandpa_proveFinality",
"params": [null, null, null],
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
