---
description: >-
  Example code for the sendrawtransaction json-rpc method. Сomplete guide on how
  to use sendrawtransaction json-rpc in GetBlock.io Web3 documentation.
---

# sendrawtransaction - Bitcoin

#### Parameters

`hexstring` - string, required

The hex string of the raw transaction

`maxfeerate` - numeric or string, optional, default=0.10

Reject transactions whose fee rate is higher than the specified value, expressed in BTC/kB.

Set to 0 to accept any fee rate.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/v1/mainnet/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "sendrawtransaction",
"params": ["hexstring", null],
"id": "getblock.io"}'
```

#### Response

```java
null
```
