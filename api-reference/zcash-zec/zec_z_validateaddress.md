---
description: >-
  Example code for the z_validateaddress json-rpc method. Сomplete guide on how
  to use z_validateaddress json-rpc in GetBlock.io Web3 documentation.
---

# z\_validateaddress - Zcash

#### Parameters

`zaddr` - string

The z address to validate.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "z_validateaddress",
"params": ["t1L2rjgGrvEqfrA5zqUca4GGxAeQg47CTpG"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": {
        "isvalid": false
    }
}
```
