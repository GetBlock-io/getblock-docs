---
description: >-
  Example code for the validateaddress json-rpc method. Сomplete guide on how to
  use validateaddress json-rpc in GetBlock.io Web3 documentation.
---

# validateaddress - Zcash

#### Parameters

`zcashaddress` - string

The Zcash address to validate

#### Request

```java
curl --location --request POST 'https://zec.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "validateaddress",
"params": ["t1L2rjgGrvEqfrA5zqUca4GGxAeQg47CTpG"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": {
        "address": "t1L2rjgGrvEqfrA5zqUca4GGxAeQg47CTpG",
        "ismine": false,
        "isscript": false,
        "isvalid": true,
        "iswatchonly": false,
        "scriptPubKey": "76a91417b04a8ede7164eccb961f46289305ec04014b6388ac"
    }
}
```
