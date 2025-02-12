---
description: >-
  Example code for the eth_getTransactionCount json-rpc method. Сomplete guide
  on how to use eth_getTransactionCount json-rpc in GetBlock.io Web3
  documentation.
---

# eth\_getTransactionCount - Cronos

#### Parameters

`data` - hex string

20-byte account address.

`quantity|tag` - hex string

Integer representing a block number or one of the string tags latest, earliest, or pending, as described in Block Parameter.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getTransactionCount",
"params": ["0x227f6757289a86c13eee2e91c2e6eb03f2ed11a6", "0x08840bf78ffc1aebc8237d3c3d209f8337f0e0c2f975e5b1c3eac816d28d760e"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "0x30751"
}
```
