---
description: >-
  Example code for the gobject_check json-rpc method. Сomplete guide on how to
  use gobject_check json-rpc in GetBlock.io Web3 documentation.
---

# gobject\_check - Dash

#### Parameters

`method name` - string

None

`data-hex` - string (hex)

The data (hex) of a governance proposal object.

#### Request

```java
curl --location --request POST 'https://dash.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "gobject",
"params": ["check", "7b22656e645f65706f6368223a313633393039383431352c226e616d65223a224272617a696c513432303231222c227061796d656e745f61646472657373223a225871746671736f44663254376e636842394e55716b71796b766f777a51684c684150222c227061796d656e745f616d6f756e74223a3131302c2273746172745f65706f6368223a313632383739363439352c2274797065223a312c2275726c223a2268747470733a2f2f7777772e6461736863656e7472616c2e6f72672f702f446173684272617a696c513432303231227d"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": {
        "Object status": "OK"
    }
}
```
