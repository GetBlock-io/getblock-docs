---
description: >-
  Example code for the get_block_template json-rpc method. Сomplete guide on how
  to use get_block_template json-rpc in GetBlock.io Web3 documentation.
---

# get\_block\_template - Monero

#### Parameters

`wallet_address` - string

Address of wallet to receive coinbase transactions if block is successfully mined.

`reserve_size` - unsigned int

Reserve size.

#### Request

```java
curl --location --request POST 'https://xmr.getblock.io/mainnet/json_rpc' \ 
--header 'x-api-key: YOUR-API-KEY' \ 
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "get_block_template",
"params": {"wallet_address": "44GBHzv6ZyQdJkjqZje6KLZ3xSyN1hBSFAnLP6EAqJtCRVzMzZmeXTC2AHKDS9aEDTRKmo6a6o9r9j86pYfhCWDkKjbtcns", "reserve_size": 60},
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "blockhashing_blob": "0e0ed99ef28606c5f1a60c9ce19bf08904f5ad338ce2d0b2a86e03911da37659ecb2e55d2d77680000000067325f8b86fba49dbf54c80f42ae0c2f6cbe76f24e1a9ea2f72f1cfdf1d4fb710e",
        "blocktemplate_blob": "0e0ed99ef28606c5f1a60c9ce19bf08904f5ad338ce2d0b2a86e03911da37659ecb2e55d2d776800000000029294920101ffd693920101dc95dde3fd1b027d57fd4671045be441fcdcbb4f779479ad58935275322068e21eff79393db4895f01041f58e141932977a8129faa1ec642c8c69c8b55469c4a27cb1f436db133635f023c000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000da655d28f2894f5d7abe41a919522bf9a75a924f85a034775eed39b77bcdf4325cf39150e25f6752b1e90d114ceb1ce143f3a12328fd64143c3d8f0069edd7f34b2e6d19a0967c01b565ca151f1298d3cd277829f1f6810b848a7bacb9ced551591770b217087695783a86eccb153742e5a8a4d090c1c10960e15a3bb41d96b522997d4d338c51b00aea98a170df4aa01b03cd923f427f6742ce1229c857a89ddc4a8d4ce06b7b3670695221530bff0611d30d84973c0d2ae252a727c612cd1531909feb34bb6fc5725913101197ee57b9e4141dedbf04a289c5f8ff7aec937bb1c00751c12d532e0405dd320723857cb1ce139d7a14355294a0eeb9cb3f699f1de7e223cfb2a1e92d4f9a0ca7eec8c834c0647f2d1f9f7d9dec95b5cebd7ec9eb498d8ef77e29d027d47c96128e1d3ddf44e7535cd9aa71cbbca5f6b47e037576e904073ce2a7f8febac082e72f68f5b463898adc17909135caff9639bc2a86d461c6f1b673aadd9fd2e681769ab93a6b9fea019c8b91ba74b1016f840446dc1a17161ceac3b4d1b07e01116933b20c879ce5d80b3b43b57f28a9e2ee7f000c6",
        "difficulty": 294558564173,
        "difficulty_top64": 0,
        "expected_reward": 961476512476,
        "height": 2394582,
        "next_seed_hash": "",
        "prev_hash": "c5f1a60c9ce19bf08904f5ad338ce2d0b2a86e03911da37659ecb2e55d2d7768",
        "reserved_offset": 130,
        "seed_hash": "4149748e079b0c223827348d4602e92da4f555b7ad53d2a98515d512cb2d0bae",
        "seed_height": 2394112,
        "status": "OK",
        "untrusted": false,
        "wide_difficulty": "0x44950eff4d"
    }
}
```
