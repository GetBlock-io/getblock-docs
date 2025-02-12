---
description: >-
  Example code for the sendrawtransaction json-rpc method. Сomplete guide on how
  to use sendrawtransaction json-rpc in GetBlock.io Web3 documentation.
---

# sendrawtransaction - Dash

#### Parameters

`Transaction` - string (hex)

The serialized transaction to broadcast encoded as hex.

`Allow High Fees` - bool

Optional.

Set to true to allow the transaction to pay a high transaction fee. Set to false (the default) to prevent Dash Core from broadcasting the transaction if it includes a high fee.

Transaction fees are the sum of the inputs minus the sum of the outputs, so this high fees check helps ensures user including a change address to return most of the difference back to themselves.

`Use InstantSend` - bool

Optional.

Deprecated and ignored since Dash Core 0.15.0

`Bypass Limits` - bool

Optional.

Bypass transaction policy limits

#### Request

```java
curl --location --request POST 'https://dash.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "sendrawtransaction",
"params": [null, "03000500010000000000000000000000000000000000000000000000000000000000000000ffffffff2703716d170423ce39610800004440830900000fe4b883e5bda9e7a59ee4bb99e9b1bc04f09f909f40440fa802203d5807000000001976a9147c086eada12bdb10a265c16c08a7ae87366bd48188aca03c9f08000000001976a91406c7111117f7b797528485b64772d3ffcff919ec88ac209af41f460200716d1700efc371b5251f5bae393e5962fe092f8b2003732a56eda3e1a2babe8413d17ce7ce2396a41c1f833c0cd00a0d8e900dfc4962805706e70a35074dcd30fafbd4c6", null, true],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": "2f124cb550d9967b81914b544dea3783de23e85d67a9816f9bada665ecfe1cd5"
}
```
