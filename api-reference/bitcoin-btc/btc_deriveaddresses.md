---
description: >-
  Example code for the deriveaddresses json-rpc method. Сomplete guide on how to
  use deriveaddresses json-rpc in GetBlock.io Web3 documentation.
---

# deriveaddresses - Bitcoin

#### Parameters

`descriptor` - string, required

The descriptor.

`range` - numeric or array, optional

If a ranged descriptor is used, this specifies the end or the range (in \[begin,end] notation) to derive.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/v1/mainnet/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "sendrawtransaction",
"params": ["descriptor", null],
"id": "getblock.io"}'
```
