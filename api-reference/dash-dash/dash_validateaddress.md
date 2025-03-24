---
description: >-
  Example code for the validateaddress json-rpc method. Сomplete guide on how to
  use validateaddress json-rpc in GetBlock.io Web3 documentation.
---

# validateaddress - Dash

#### Parameters

`Address` - string (base58)

The P2PKH or P2SH address to validate encoded in base58check format.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "validateaddress",
"params": ["7n9YufoWBrJ65YC7sgwk53EzbPHMQRB6HJ"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": {
        "address": "7n9YufoWBrJ65YC7sgwk53EzbPHMQRB6HJ",
        "isscript": true,
        "isvalid": true,
        "scriptPubKey": "a914d887657080297c7ee1d2faf3970017c82bdf8b5487"
    }
}
```
