---
description: >-
  Example code for the getblock json-rpc method. Сomplete guide on how to use
  getblock json-rpc in GetBlock.io Web3 documentation.
---

# getblock - Dogecoin

#### Parameters

`blockhash` - string

Hash of a block.

#### Request

```java
curl --location --request POST 'https://doge.getblock.io/mainnet/' \
--header 'x-api-key: YOUR-API-KEY' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "getblock",
"params": ["24f64485ea05250ce75a73b3549afad05e078a7d8abd54e695a957dadee264fa"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": {
        "auxpow": {
            "chainindex": 0,
            "chainmerklebranch": [],
            "index": 0,
            "merklebranch": [
                "7ccddc30a8522baf907e9f8f4d98b966a8ae62e24bdbc3cce6bdd49da62db988",
                "15a8049cd4c38bafcde0e15cc6ab8aa228db2b771c810cd9b5acebd6d2edcefe",
                "48e3746c8fe36310c84838664886fd6b74c5890a6a2c9fdbde35eb66dc6377d6",
                "4d63f600c33496bddafc2ba3da1eabf17e3a9494952f380726798b5a78f4cf74",
                "8034987d58032dfddad651e7c577384b53a33e964d4eb7724f49673137487949",
                "ee6d298549659f4c0058b248f43a8726b74316071d1388220a2011272c3b454f",
                "8acce20b7730d9508a7025398119c889db27924503d6925277087841abdc483f",
                "8388f109347514201668e59057327b4ccf1f3ed02206186de1921fe2a4cdf5a7",
                "1f360e6b4d7b3e497125e802c3f685991d5e584d4d87f01c2bb1fb25d5b44ec4",
                "c0d15031723fe7d26ae3388dbab21ebf71a448cdf491d51a0893cf875e707267"
            ],
            "parentblock": "000000207a9ac39b6742136394e619a81a234c8b75caa2e2f6d187a9fb1e00698ff12e37ed24b3b87dc5d7defa32b79b906f76772b3df83f9ad3d68d6d62a530f41dcf1d4b6e48618a87011a17e39e10",
            "tx": {
                "blockhash": "e60fd6dce484786fa8a1caefbf76d798c0a8aa64becb9897cf41b7aa4a251233",
                "hash": "10c03013c4feadd7d2e0d948240771dbbc220152d12c3ab7628a40a7f827f658",
                "hex": "01000000010000000000000000000000000000000000000000000000000000000000000000ffffffff5203507120194d696e656420627920416e74506f6f6c2070001502254b4138fabe6d6d24f64485ea05250ce75a73b3549afad05e078a7d8abd54e695a957dadee264fa010000000000000031cc000003000000ffffffff0274ca594b000000001976a914a5f4d12ce3685781b227c1f39548ddef429e978388ac0000000000000000266a24aa21a9ed5cf6b111db7da5a08b4b01db77d626bc9e57d8ece4f6cc7315ecad6079124eac00000000",
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
        },
        "bits": "1a04c252",
        "chainwork": "0000000000000000000000000000000000000000000005c2f27be4a1f414f1c0",
        "confirmations": 1389,
        "difficulty": 3525264.838757254,
        "hash": "24f64485ea05250ce75a73b3549afad05e078a7d8abd54e695a957dadee264fa",
        "height": 3904788,
        "mediantime": 1632136417,
        "merkleroot": "8e992e1512887da5e02fd52a1bd19d527ee54436934c215f0c23cf4ec04771e7",
        "nextblockhash": "b53179ba74966931249faf735fc49af1b1ad686f8b12bb3ae6dcd7a35243e358",
        "nonce": 0,
        "previousblockhash": "f9e8b36d03890feae429b2810c086683dca8eec65c5b7e0ca9fca7a4540fd1f4",
        "size": 828,
        "strippedsize": 828,
        "time": 1632136777,
        "tx": [
            "8e992e1512887da5e02fd52a1bd19d527ee54436934c215f0c23cf4ec04771e7"
        ],
        "version": 6422788,
        "versionHex": "00620104",
        "weight": 3312
    }
}
```
