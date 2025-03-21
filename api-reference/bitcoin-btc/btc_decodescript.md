---
description: >-
  Example code for the decodescript json-rpc method. Сomplete guide on how to
  use decodescript json-rpc in GetBlock.io Web3 documentation.
---

# decodescript - Bitcoin

#### Parameters

`hexstring` - string, required

the hex-encoded script

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/v1/mainnet/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "decodescript",
"params": ["hexstring"],
"id": "getblock.io"}'
```

#### Response

```java
null
```
