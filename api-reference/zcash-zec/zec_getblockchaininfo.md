---
description: >-
  Example code for the getblockchaininfo json-rpc method. Сomplete guide on how
  to use getblockchaininfo json-rpc in GetBlock.io Web3 documentation.
---

# getblockchaininfo - Zcash

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "getblockchaininfo",
"params": [],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": {
        "bestblockhash": "00000000019af1612080b8d6b3ce87c5b7569a114e6a77385260cf45a6419fe1",
        "blocks": 1384358,
        "chain": "main",
        "chainwork": "00000000000000000000000000000000000000000000000006cad1298bfcc257",
        "commitments": 1544318,
        "consensus": {
            "chaintip": "e9ff75a6",
            "nextblock": "e9ff75a6"
        },
        "difficulty": 48487005.99738715,
        "estimatedheight": 1384358,
        "headers": 1384358,
        "initial_block_download_complete": true,
        "pruned": false,
        "size_on_disk": 31423699844,
        "softforks": [
            {
                "enforce": {
                    "found": 4000,
                    "required": 750,
                    "status": true,
                    "window": 4000
                },
                "id": "bip34",
                "reject": {
                    "found": 4000,
                    "required": 950,
                    "status": true,
                    "window": 4000
                },
                "version": 2
            },
            {
                "enforce": {
                    "found": 4000,
                    "required": 750,
                    "status": true,
                    "window": 4000
                },
                "id": "bip66",
                "reject": {
                    "found": 4000,
                    "required": 950,
                    "status": true,
                    "window": 4000
                },
                "version": 3
            }
        ],
        "upgrades": {
            "2bb40e60": {
                "activationheight": 653600,
                "info": "See https://z.cash/upgrade/blossom/ for details.",
                "name": "Blossom",
                "status": "active"
            },
            "5ba81b19": {
                "activationheight": 347500,
                "info": "See https://z.cash/upgrade/overwinter/ for details.",
                "name": "Overwinter",
                "status": "active"
            },
            "76b809bb": {
                "activationheight": 419200,
                "info": "See https://z.cash/upgrade/sapling/ for details.",
                "name": "Sapling",
                "status": "active"
            }
        },
        "valuePools": [
            {
                "chainValue": 29342.43164115,
                "chainValueZat": 2934243164115,
                "id": "sprout",
                "monitored": true
            },
            {
                "chainValue": 844070.82117579,
                "chainValueZat": 84407082117579,
                "id": "sapling",
                "monitored": true
            }
        ],
        "verificationprogress": 0.9999989219382445
    }
}
```
