---
description: >-
  Example code for the sendrawtransaction json-rpc method. Сomplete guide on how
  to use sendrawtransaction json-rpc in GetBlock.io Web3 documentation.
---

# sendrawtransaction - Bitcoin Gold

#### Parameters

`hexstring` - string, required

The hex string of the raw transaction

`maxfeerate` - numeric or string, optional, default=0.10

Reject transactions whose fee rate is higher than the specified value, expressed in BTC/kB.

Set to 0 to accept any fee rate.

#### Request

```java
curl --location --request POST 'https://btg.getblock.io/mainnet' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "sendrawtransaction",
"params": ["010000000001010000000000000000000000000000000000000000000000000000000000000000ffffffff320336b80a005a2d4e4f4d50212068747470733a2f2f6769746875622e636f6d2f6a6f7368756179616275742f7a2d6e6f6d70ffffffff0240be4025000000001976a9140cb60a52559620e5de9a297612d49f55f7fd14ea88ac0000000000000000266a24aa21a9ede2f61c3f71d1defd3fa999dfa36953755c690689799962b48bebd836974e8cf90120000000000000000000000000000000000000000000000000000000000000000000000000", null],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": null
}
```
