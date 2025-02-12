---
description: >-
  Example code for the hmy_newFilter json-rpc method. Сomplete guide on how to
  use hmy_newFilter json-rpc in GetBlock.io Web3 documentation.
---

# hmy\_newFilter - Harmony

#### Parameters

`Object` - object

Filter options

#### Request

```java
curl --location --request POST 'https://one.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "hmy_newFilter",
"params": [{"topics": ["0xb7407a6d74c6472bf99c4c9abe0860dc439469421c42c060639733b2309b05c7"]}],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "0x6774cfbc91febb08e60bf2f2a0ca8cb8"
}
```
