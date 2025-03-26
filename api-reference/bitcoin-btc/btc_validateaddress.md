---
description: >-
  Example code for the validateaddress json-rpc method. Сomplete guide on how to
  use validateaddress json-rpc in GetBlock.io Web3 documentation.
---

# validateaddress - Bitcoin

#### Parameters

`address` - string, required

The btc address to validate

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "validateaddress",
"params": ["bc1q09vm5lfy0j5reeulh4x5752q25uqqvz34hufdl"],
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
