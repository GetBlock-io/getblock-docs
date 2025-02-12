---
description: >-
  Example code for the /getShardBlockProof json-rpc method. Сomplete guide on
  how to use /getShardBlockProof json-rpc in GetBlock.io Web3 documentation.
---

# /getShardBlockProof - The Open Network (TON)

#### Parameters

`workchain` - query

required, integer

block workchain id

`shard` - query

required, integer

block shard id

`seqno` - query

required, integer

block seqno

`from_seqno` - query

optional, integer

Seqno of masterchain block starting from which proof is required. If not specified latest masterchain block is used.

#### Request

```java
curl --location --request GET 'https://ton.getblock.io/mainnet/getShardBlockProof?workchain=-1&shard=-9223372036854775808&seqno=30497145' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json'
```

#### Response

```java
{
    "ok": true,
    "result": {
        "@extra": "1687338544.8136253:0:0.792949744681948",
        "@type": "blocks.shardBlockProof",
        "from": {
            "@type": "ton.blockIdExt",
            "file_hash": "yhFiArzGTVG04DNU0L+usdfOCwmpEFCjvouHKUluZ0I=",
            "root_hash": "SV/NmAOmNTzjkuJO9uyroF7jv0KEiJyNFd34Ye5AZsY=",
            "seqno": 30517698,
            "shard": "-9223372036854775808",
            "workchain": -1
        },
        "links": [],
        "mc_id": {
            "@type": "ton.blockIdExt",
            "file_hash": "t81TFPL17RLgqteZsYf64yY2EX18p/V5f3Z9itK1DrA=",
            "root_hash": "2a8ZfAc+YN+hWDxtnlj+lsmPxDljA1ur8XVvL52UB2s=",
            "seqno": 30497145,
            "shard": "-9223372036854775808",
            "workchain": -1
        },
        "mc_proof": [
            {
                "@type": "blocks.blockLinkBack",
                "dest_proof": "te6ccgECBwEAAUcACUYD2a8ZfAc+YN+hWDxtnlj+lsmPxDljA1ur8XVvL52UB2sAFgEkEBHvVar///8RAgMEBQGgm8ephwAAAAAEAQHRWXkAAAABAP////8AAAAAAAAAAGSRt6IAACMj2TWowAAAIyPZNajEIo3AWgAG3EgB0Vl1AdE5dMQAAAADAAAAAAAAAC4GKEgBAcbfDt4EasCGjPemonECEdBmFZHcjpVWsJUjbC3Tox0vAAMoSAEBZBsNxHAgiuaVY6i23SfCghiMAlUqoDQC0/YeayFULc4AFShIAQEU7xkWEeDam3oBjCjcCaf89NzbXoY2u5ZHYxRItDQGrwAHAJgAACMj2SZmhAHRWXiLzgJ0H2JOUL+uzLeQEb5EXdNNYjIbfnwvEeBoRsLGQP1wlslyGVcvgYNwMSOZJtCD+m57JpmyPW0kAztPZty8",
                "from": {
                    "@type": "ton.blockIdExt",
                    "file_hash": "yhFiArzGTVG04DNU0L+usdfOCwmpEFCjvouHKUluZ0I=",
                    "root_hash": "SV/NmAOmNTzjkuJO9uyroF7jv0KEiJyNFd34Ye5AZsY=",
                    "seqno": 30517698,
                    "shard": "-9223372036854775808",
                    "workchain": -1
                },
                "proof": "te6ccgECCQEAAfoACUYDSV/NmAOmNTzjkuJO9uyroF7jv0KEiJyNFd34Ye5AZsYAFgEkEBHvVar///8RAgMEBQGgm8ephwAAAAAEAQHRqcIAAAABAP////8AAAAAAAAAAGSSviEAACMoxs0wQAAAIyjGzTBEX71EWwAG3VcB0am9AdGHkcQAAAADAAAAAAAAAC4GKEgBAWMqeBZiIZ+BquwyDeAF+7RsNF8GxENzi5Ehggz7w7trAAMqigT/kRHeYUV9yOW6VS+I4ertCLrfza4DE2ZbwGDy375e60q5VRCujBM/LFFxtPiSlyMARs/Zce/TT8MSmrXj2tCCAW8BbwcIKEgBAcYmH6W+YAWl3VAQsLkfOO2sulzQjJfNyXel/0DrUgWKAAcAmAAAIyjGve4EAdGpwaffN1U4iqelPtYnIVOhNGqUqYn2MvScC4nyFkzmRyqX3xTonEnEtJWVAywzPykQRj2851JicjkAGScG2R2IoodojAED/5ER3mFFfcjlulUviOHq7Qi6382uAxNmW8Bg8t++Xuvoppc1TfCzjsduM2Keh8Mg79sEvyf6cwm1o2FF7h5TCQFvABRojAEDSrlVEK6MEz8sUXG0+JKXIwBGz9lx79NPwxKatePa0IJ6EULU7xRi3h2hnzwh8yy6hgD29f9GJ2Z8xMIxtt7zzQFvABQ=",
                "state_proof": "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",
                "to": {
                    "@type": "ton.blockIdExt",
                    "file_hash": "t81TFPL17RLgqteZsYf64yY2EX18p/V5f3Z9itK1DrA=",
                    "root_hash": "2a8ZfAc+YN+hWDxtnlj+lsmPxDljA1ur8XVvL52UB2s=",
                    "seqno": 30497145,
                    "shard": "-9223372036854775808",
                    "workchain": -1
                },
                "to_key_block": false
            }
        ]
    }
}
```
