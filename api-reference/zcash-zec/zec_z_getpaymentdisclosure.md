---
description: >-
  Example code for the z_getpaymentdisclosure  {disallowed} json-rpc method.
  Сomplete guide on how to use z_getpaymentdisclosure  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# z\_getpaymentdisclosure {disallowed} - Zcash

#### Parameters

`txid` - string

None

`js_index` - string

None

`output_index` - string

None

`message` - string

Optional

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "z_getpaymentdisclosure",
"params": [null, null, null, null],
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
