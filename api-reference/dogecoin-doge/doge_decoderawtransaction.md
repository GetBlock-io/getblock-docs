---
description: >-
  Example code for the decoderawtransaction json-rpc method. Сomplete guide on
  how to use decoderawtransaction json-rpc in GetBlock.io Web3 documentation.
---

# decoderawtransaction - Dogecoin

#### Parameters

`hexstring` - string

A hex string of the transaction to be decoded.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "decoderawtransaction",
"params": ["01000000010000000000000000000000000000000000000000000000000000000000000000ffffffff5203507120194d696e656420627920416e74506f6f6c2070001502254b4138fabe6d6d24f64485ea05250ce75a73b3549afad05e078a7d8abd54e695a957dadee264fa010000000000000031cc000003000000ffffffff0274ca594b000000001976a914a5f4d12ce3685781b227c1f39548ddef429e978388ac0000000000000000266a24aa21a9ed5cf6b111db7da5a08b4b01db77d626bc9e57d8ece4f6cc7315ecad6079124eac00000000"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": {
        "hash": "10c03013c4feadd7d2e0d948240771dbbc220152d12c3ab7628a40a7f827f658",
        "locktime": 0,
        "size": 214,
        "txid": "10c03013c4feadd7d2e0d948240771dbbc220152d12c3ab7628a40a7f827f658",
        "version": 1,
        "vin": [
            {
                "coinbase": "03507120194d696e656420627920416e74506f6f6c2070001502254b4138fabe6d6d24f64485ea05250ce75a73b3549afad05e078a7d8abd54e695a957dadee264fa010000000000000031cc000003000000",
                "sequence": 4294967295
            }
        ],
        "vout": [
            {
                "n": 0,
                "scriptPubKey": {
                    "addresses": [
                        "DLGbK6mCjBT67r8wjJqCg8hkFiBYV5JquH"
                    ],
                    "asm": "OP_DUP OP_HASH160 a5f4d12ce3685781b227c1f39548ddef429e9783 OP_EQUALVERIFY OP_CHECKSIG",
                    "hex": "76a914a5f4d12ce3685781b227c1f39548ddef429e978388ac",
                    "reqSigs": 1,
                    "type": "pubkeyhash"
                },
                "value": 12.64175732
            },
            {
                "n": 1,
                "scriptPubKey": {
                    "asm": "OP_RETURN aa21a9ed5cf6b111db7da5a08b4b01db77d626bc9e57d8ece4f6cc7315ecad6079124eac",
                    "hex": "6a24aa21a9ed5cf6b111db7da5a08b4b01db77d626bc9e57d8ece4f6cc7315ecad6079124eac",
                    "type": "nulldata"
                },
                "value": 0.0
            }
        ],
        "vsize": 214
    }
}
```
