---
description: >-
  Example code for the eth_getProof json-rpc method. Сomplete guide on how to
  use eth_getProof json-rpc in GetBlock.io Web3 documentation.
---

# eth\_getProof - Ethereum Classic

#### Parameters

`DATA` - string

20-byte address of the account or contract.

`ARRAY` - array of string

Array of 32-byte storage keys to generate proofs for.

`QUANTITY|TAG` - string

Integer representing a block number or one of the string tags latest, earliest, or pending, as described in Block Parameter.

#### Request

```java
curl --location --request POST 'https://etc.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getProof",
"params": ["0a8156e7ee392d885d10eaa86afd0e323afdcd95", [[839]], "latest"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "accountProof": [
            "0xf90211a0...608d898380",
            "0xf90211a0...ec33f19580",
            "0xf901d1a0...9e55584480",
            "0xf8718080...18e5777142"
        ],
        "address": "0x0a8156e7ee392d885d10eaa86afd0e323afdcd95",
        "balance": "0x0",
        "codeHash": "0x2b6975dcaf69f9bb9a3b30bb6a37b305ce440250bf0dd2f23338cb18e5777142",
        "nonce": "0x5f",
        "storageHash": "0x917688de43091589aa58c1dfd315105bc9de4478b9ba7471616a4d8a43d46203",
        "storageProof": [
            {
                "key": "0x0000000000000000000000000000000000000000000000000000000000000347",
                "value": "0x0",
                "proof": [
                    "0xf90211a0...5176779280",
                    "0xf901f1a0...c208d86580",
                    "0xf8d180a0...1ce6808080"
                ]
            }
        ]
    }
}
```
