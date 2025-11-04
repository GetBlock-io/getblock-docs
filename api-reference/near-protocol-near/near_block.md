---
description: >-
  Example code for the block json-rpc method. Сomplete guide on how to use block
  json-rpc in GetBlock.io Web3 documentation.
---

# block - NEAR Protocol
This method returns detailed information about a specific block on the NEAR blockchain. You can query a block by block ID (height), hash, and finality (e.g., final or optimistic).

## Supported Network
  - Mainnet

## Parameters
| Parameter    | Type             | Required | Description                                                                              |
| ------------ | ---------------- | -------- | ---------------------------------------------------------------------------------------- |
| `id` | string | Optional | The height or hash of the block you want to query.                                       |
| `finality` | string           | Required | Specifies which finalized block to return. Acceptable values: `"final"`, `"optimistic"`, `"near-final"`. |
| `method`   | string           | Required | Always set to `"block"`.                                                                 |


## Request

- **URL (endpoint)**

    ```bash
    https://go.getblock.io/
    ```
    
- **Example (cURL)**
    ```java
    curl --location 'https://go.getblock.io/<ACCESS_TOKEN>' \
    --header 'Content-Type: application/json' \
    --data '{"jsonrpc": "2.0",
    "method": "block",
    "params": {"finality": "final"},
    "id": "17821130"}'
    ```

## Response

```java
{
    "jsonrpc": "2.0",
    "result": {
        "author": "luganodes.pool.near",
        "chunks": [
            {
                "balance_burnt": "134110621882500000000",
                "bandwidth_requests": {
                    "V1": {
                        "requests": []
                    }
                },
                "chunk_hash": "DuZtgqkXV4zpVez4p4dgbqgXbceUoQCsdGGN1gUujcbF",
                "congestion_info": {
                    "allowed_shard": 9,
                    "buffered_receipts_gas": "0",
                    "delayed_receipts_gas": "0",
                    "receipt_bytes": 0
                },
                "encoded_length": 1102,
                "encoded_merkle_root": "DeZ7YNe3KSnvoPcE1wdWBeCSWuJabx8CZYAZAVyzkgCx",
                "gas_limit": 1000000000000000,
                "gas_used": 2010653906325,
                "height_created": 169553183,
                "height_included": 169553183,
                "outcome_root": "4G1TxuKdWKzMkc62QzwzFYtv7owKpWRunvrzqBep5HDX",
                "outgoing_receipts_root": "GJr1Us3FtdoZVRpUYpSJ1Dq9cHppNjFFQAskQxepzTFM",
                "prev_block_hash": "G7SfPwTMoAtrfpqdpv2ZpC4HnD8g4iQE9c3iqLRJobwa",
                "prev_state_root": "7otHB6vns5YNgZKpzVBo55ov5T7vrv9fndZ7Fnpf3z9Z",
                "rent_paid": "0",
                "shard_id": 10,
                "signature": "ed25519:4e544LFveErqKMuVz5C9vnudcpiZzaSi5r1s7jgFsQSJRj4DJpPjLHrpDTkqAqE7NRH1cYdsQBb5URY2eR3Zx1WV",
                "tx_root": "11111111111111111111111111111111",
                "validator_proposals": [],
                "validator_reward": "0"
            },
            {
                "balance_burnt": "717904790250000000000",
                "bandwidth_requests": {
                    "V1": {
                        "requests": []
                    }
                },
                "chunk_hash": "7X6BhMtfZE8mYzmMP24pih98ezzrwZUwjAT1G2aJCDbD",
                "congestion_info": {
                    "allowed_shard": 6,
                    "buffered_receipts_gas": "0",
                    "delayed_receipts_gas": "0",
                    "receipt_bytes": 0
                },
                "encoded_length": 2237,
                "encoded_merkle_root": "C72mKNSpbtspkh39vZPD5EeVsA4eFvVz42eiBKex4m8U",
                "gas_limit": 1000000000000000,
                "gas_used": 7179047902500,
                "height_created": 169553183,
                "height_included": 169553183,
                "outcome_root": "AMu5Y4Afjrp6u6g8Z4h6kaCYkdbkR6gY9oSiVWHPFMum",
                "outgoing_receipts_root": "9MkKLB1XPgVr192NRPpUwNhAwpBCVSHK7UVkL4b3raNr",
                "prev_block_hash": "G7SfPwTMoAtrfpqdpv2ZpC4HnD8g4iQE9c3iqLRJobwa",
                "prev_state_root": "7ypt5RKeEnFuvgiXneVitUmGS9C8YoopHc3kJ1T7AgKx",
                "rent_paid": "0",
                "shard_id": 11,
                "signature": "ed25519:4hsg7U2tEwFUUYfdanTS7ZPyERzJFiwJYkonsqdwdp27jB2b6U96L3rSWgD2YAzJgLvK3amGfc9jiCqqoLzrfbbs",
                "tx_root": "11111111111111111111111111111111",
                "validator_proposals": [],
                "validator_reward": "0"
            },
            {
                "balance_burnt": "343589020267200000000",
                "bandwidth_requests": {
                    "V1": {
                        "requests": []
                    }
                },
                "chunk_hash": "DtSeuwAyBnfZGCqhx381Ef43yM73rne3SCHFsE3xi2KR",
                "congestion_info": {
                    "allowed_shard": 4,
                    "buffered_receipts_gas": "0",
                    "delayed_receipts_gas": "0",
                    "receipt_bytes": 0
                },
                "encoded_length": 552,
                "encoded_merkle_root": "5FCiKTzYo5nEDDAdpbWCXGH92R4oEGJEE5ZUC4yAj8rn",
                "gas_limit": 1000000000000000,
                "gas_used": 4146871912398,
                "height_created": 169553183,
                "height_included": 169553183,
                "outcome_root": "79ypwdoWvhUUXDSERuEwokeKd81MkxBXofBfF7VfrGBs",
                "outgoing_receipts_root": "EbY2RzJ5zYfHHCKhMUCc1yappAtFvyb9GpRW6nGcd8RP",
                "prev_block_hash": "G7SfPwTMoAtrfpqdpv2ZpC4HnD8g4iQE9c3iqLRJobwa",
                "prev_state_root": "5889A3ntTM8u5qXQuvCfPFDmZpgsFwc6Vv31Szk5pwtP",
                "rent_paid": "0",
                "shard_id": 8,
                "signature": "ed25519:52vL7fU8yMc9qL2MPkUeCMxVayFrajHGnrNqTBCXEAHtj9Gwzu3rakyHRF6r2HyTaxk6uZJUohdAEc6j86Jk4Ckr",
                "tx_root": "5dZjfRuj6z7rSfLTrYvRco7eW3XuhaSiZpc4wnFSZdg4",
                "validator_proposals": [],
                "validator_reward": "0"
            },
            {
                "balance_burnt": "83175925852000000000",
                "bandwidth_requests": {
                    "V1": {
                        "requests": []
                    }
                },
                "chunk_hash": "AtsREVVDVxB93nwjz2t8jUqL3M7pBF56FV3zzXGeZoge",
                "congestion_info": {
                    "allowed_shard": 11,
                    "buffered_receipts_gas": "0",
                    "delayed_receipts_gas": "0",
                    "receipt_bytes": 0
                },
                "encoded_length": 689,
                "encoded_merkle_root": "27mswh7hBzbPvagXiYeg9SBPTsNFRfHNBpvmVHSBAhDz",
                "gas_limit": 1000000000000000,
                "gas_used": 1054941821020,
                "height_created": 169553183,
                "height_included": 169553183,
                "outcome_root": "tEyw1Fjeba7P85noJcecdskouPYcGPKpzoW2LaaQmtT",
                "outgoing_receipts_root": "43u7wL9XxEmmqXmVsG7Yc4K6sFp8PYX6qyxNvm6vu2ch",
                "prev_block_hash": "G7SfPwTMoAtrfpqdpv2ZpC4HnD8g4iQE9c3iqLRJobwa",
                "prev_state_root": "FcPVHjQymFC8AnWZpt5gWf3MoSMXZUQPzyiBiUFHUhEc",
                "rent_paid": "0",
                "shard_id": 7,
                "signature": "ed25519:jYSvM5bdkx2pwkhuoaLedXznQoKQJBsSCMmuXhvPj2BqdtFFQCbU3KvVJHcpjJR1FT3pH6RsLBYDhQ7z7mA6VAz",
                "tx_root": "11111111111111111111111111111111",
                "validator_proposals": [],
                "validator_reward": "0"
            },
            {
                "balance_burnt": "2227798371839700000000",
                "bandwidth_requests": {
                    "V1": {
                        "requests": []
                    }
                },
                "chunk_hash": "9rPbL8ggENjWEUoj4EFtpS2FLdjBezZS6kT2vjf3wDCD",
                "congestion_info": {
                    "allowed_shard": 8,
                    "buffered_receipts_gas": "0",
                    "delayed_receipts_gas": "0",
                    "receipt_bytes": 0
                },
                "encoded_length": 8124,
                "encoded_merkle_root": "5Vuo6WoBz6K7Afo862C4n6oLPDoG2CguERHVHqEegGB4",
                "gas_limit": 1000000000000000,
                "gas_used": 26606917035764,
                "height_created": 169553183,
                "height_included": 169553183,
                "outcome_root": "69gVWHD6mHBQGYKy3ZkYnmAWY2mx4kVUTtrpjmJ8aUEd",
                "outgoing_receipts_root": "EvyKDtq9CEZVeD4bgqevwkz2ptb664cNzTvPq3VY6Rc",
                "prev_block_hash": "G7SfPwTMoAtrfpqdpv2ZpC4HnD8g4iQE9c3iqLRJobwa",
                "prev_state_root": "BkfeaiMJbyAXVPAnFyfxDBogY4JYxeY8L4czeuXWrh2H",
                "rent_paid": "0",
                "shard_id": 5,
                "signature": "ed25519:21qLmgsACoyeMmM1nLP3GsGT3kXkncwquS2k4QaVGo8fNTERgA2bNZWTx6N9unQwdhQFTjhZnShBoaLbfs5ECFsx",
                "tx_root": "11111111111111111111111111111111",
                "validator_proposals": [],
                "validator_reward": "0"
            }
        ],
        "header": {
            "approvals": [
                "ed25519:2VETLZmAEcXvtLuGpUcTqZg5ARNSA3VuGRhSNPgewa6No5HaSUVFdAEXPrnzXrE5BFXLy3FHeNzYnPeSopATwt7A",
                "ed25519:2Rmpy6GAQrzSuWUbpzoNCKsxZ1xWa6vCbWoP3H11JnnGjL8Bs4gThzK7LqH7VyfnKCEdtyuqH5HaBRGibBonGySH",
                "ed25519:4G7BcHEddm1LHGudGaKKyyx2Pv69CfnntHECsB56x7y58UzsfVg4o1tBPeuMbxeJPJTCo43LMNs1oq4JREYZBVSy",
                "ed25519:3A2u8DA56EW4s8FoP3nULNnke6SQFKHifzVohTa5Ya73ALVfKd2GAGT5PrA1XGz7cakw8RWbFbNWqNYgFKeB51Po",
                "ed25519:2jVY1fGGBenDc1jhHCnMysyoJXeMDHudXec4Xh8wxSigKSKzHPYnC24zPhAnytShqU6bM7Ls5PvyevKh9ADtj8Lw",
                "ed25519:41bV9xkk1k85paULEvwwQrk3V4CKTp8q8Udg34wBvJjpoj8bHDuu5bUwZsphGeuc3bFRJ383gENahaYAZK8WZKHp",
                "ed25519:2qyADrAGYCk8ARj1q6Av32L5F2cupKziB1q7upAYU4eYSg8oaMrj4rimbMgaM49yzSG4dzFmUTPyW4jVzRYhTmLM",
                "ed25519:4SboGdJV6mDuv8HXsWgPfeyGCDmBR46jMCGzode9gkwNxZT9J6Uf2EocoVasxKmnYSEBCVYQEBcEnjvsm1E5W34g",
                "ed25519:4zzAEi4YgWA3ikve7bYvAiiK6YVrM2HPiQuyoVvrtUuBEKK7RfyeZyAqYcCtketpkmRcmTi29i45yePza8ber6uE",
                "ed25519:3FWoWzVZQ6WnJF5qnRvvLNnYMXMqtzj7c3gNTXwnN3ZJcsk8MegzXpfz3fGAxocKCSHEqXmvceDKZrYxU2VtrHfD",
                "ed25519:3YEgLh8fPnSpg4nFEkzXaNSWcxH6XEq4Vypxr1dEB6AEsYL2Bwzug1NDh8tHEqRz6DHTi4FV2G6nvDJ89ZoQy7FM",
                "ed25519:2QZGHXbTtJU1jeVKP2ugEPXx8YN98Rm94XrynEzpLrwLwaEq2ymHF3tstazb3pfDjFbVSaT6FXx45utfEtxU5LGB",
                "ed25519:3BLkMGtiZ2MoTdVhBiuW16j47cu4rm13d5aS22J8qkFxeaG9NU4aBuitRvbTDgtY8rmqZKJE27RxneCYhZSitYGV",
                "ed25519:3tHfH5dkEkCZXMWML47y6SziTbk1bZhAY6pYAXdYiuZzS84D9zy1hQjYcGsPovXfJjPYJxSCxEWZvyyYZ1a8Ph51",
                null,
                "ed25519:4fSBU8dgvTQgh69mXxqc6A2cYvNvBb7k3LbMDmdfg5RZvEtSaAzXVUzqdybbGnbWoWq48ctRU5kB6LoUxTXqk5TS",
                null,
                "ed25519:2xqhEhUy12boc5Qop2qoYoT18h2XrLkdxRqrVhuipGKoDv9msUuKyBK4JWVa38osKDahjehbH384fQh3BAzi8XQM",
                "ed25519:2maxejafFbHA8ykaPiAs2HsSTVSPCSdARrVgW7qPbSDADkNGph4qmUtMH3XecjmJ7Nmy1kqUUMkpcHAV2HQvfLdP",
                null,
                "ed25519:57bfUtuHD4YfzjUL6PAPFHnYef44PM9SxuCp9qFs2ck7qqYMzkYSgC1WbJ9jkpB3hTpe1fpLPAu9D5K2UbTdifN1",
                "ed25519:4cc1qEK7PkoYshrARKf24s4thwReujEi38FaK1BLcdDTXVUi1nizaBeM8dE2JBNjTkH25KZNvvXhWpySpbasxGjP",
                "ed25519:32jwRWQLX7V12ENFJ7wm6yHGAbCjrXPAspNJYXqffsmSfuZgQr6btA6WGCs8MHE8FtkRgqRGJPJpgqKFqHkNMw65",
                "ed25519:3Ufedkaq3HLdqRurnmsaeWkKUkuy9YVo3DU1Rivy4A2qxN4Yr3UC7kGPDuaT5NhUNWD33nebLksesMCquJfmoHHt",
                null,
                "ed25519:3t55A5sGmBtyLZbRDeoNcWjkgK7KMofHs7aPTxJXZ3HnP2NQ4WJvbBGFkizjJ5bWUdoQHs2Y85oDLnL8Qg4Hpzoc",
                "ed25519:39rcoztDkP7exKbFmRdaRLvqNbJpimyo7ow67ZkbMb4D97iJoiaV54h5xFJy66JBjnCHGB7iSPwNCkA1j6vp5TVH",
                null,
            ],
            "block_body_hash": "BgeS4Guc4BybbJ6yxjDY3dTf7D6CfBybCUHVAvkSHQmR",
            "block_merkle_root": "3pqjVQB9ixdjkWdQcvykDq2evbQgKZHQbXZ85cEhDoe2",
            "block_ordinal": 159351294,
            "challenges_result": [],
            "challenges_root": "11111111111111111111111111111111",
            "chunk_endorsements": [
                [
                    255,
                    255,
                    255,
                    223,
                    255,
                    255,
                    255,
                    255,
                    223,
                    255,
                    31
                ],
                [
                    255,
                    255,
                    255,
                    255,
                    255,
                    255,
                    255,
                    255,
                    255,
                    31
                ],
                [
                    255,
                    255,
                    255,
                    255,
                    255,
                    255,
                    247,
                    255,
                    255,
                    15
                ],
                [
                    255,
                    255,
                    255,
                    255,
                    255,
                    255,
                    255,
                    255,
                    255,
                    127,
                    15
                ]
            ],
            "chunk_headers_root": "6tLgVwEJLZPddbVsnt8xNwU4zXdiwE9LzQuTSoMcYUZr",
            "chunk_mask": [
                true,
                true,
                true,
                true,
                true,
                true,
                true,
                true,
                true
            ],
            "chunk_receipts_root": "4FHfk4hcAudmJoJqL8sUjRexCzWqWPSP9J4gydo6Reh2",
            "chunk_tx_root": "2anhkWsn56b6zVtNQmgCVoxDKjGFdtLDKQcBPBbQFjw1",
            "chunks_included": 9,
            "epoch_id": "3grCuiPFGU19eSa4xTwsvKWuM8yksXZCMrTcU7Rn8YGg",
            "epoch_sync_data_hash": null,
            "gas_price": "100000000",
            "hash": "G8KZAZF7q6x7VVmfFpe1wDijygVdz4WSscW1Sn7EPPtF",
            "height": 169553183,
            "last_ds_final_block": "G7SfPwTMoAtrfpqdpv2ZpC4HnD8g4iQE9c3iqLRJobwa",
            "last_final_block": "D1sMG4R84GArV9hutHjHhjeVvXDPSV5X1EGnREEM2T9p",
            "latest_protocol_version": 80,
            "next_bp_hash": "5BDbx9MdrqSozPyLwtfMgTaPFEJW972EeVgKzdgb497Z",
            "next_epoch_id": "7E2eB6zHzENHBN9Atq4surCt6kaQaS6Qng3XXWwB2enp",
            "outcome_root": "BUMm11tFFTZuBbYETFKBaiUQH5dkMXxG4TzguPU85ddA",
            "prev_hash": "G7SfPwTMoAtrfpqdpv2ZpC4HnD8g4iQE9c3iqLRJobwa",
            "prev_height": 169553182,
            "prev_state_root": "EMwoXPDmcueshDSTJk73zGYL5NTtSmmc46ZVadYEmD2F",
            "random_value": "23F6uVTG93Gk4yGq2GSE4s7mt2LpqERJymg7KnHv661D",
            "rent_paid": "0",
            "signature": "ed25519:SznGZNgngkz3xBQKQiR6PAuZviNutQvn4VtFiPAyPrcyFoE4cMR4ajvDsHGi1RZuWSQiR6no2ZgNx15izzJcyFR",
            "timestamp": 1761247965639797778,
            "timestamp_nanosec": "1761247965639797778",
            "total_supply": "1277588493092208736683680159652463",
            "validator_proposals": [],
            "validator_reward": "0"
        }
    },
    "id": "17821130"
}
```
## Response Definition

| **Field**                          | **Type** | **Description**                                                 |
| ---------------------------------- | -------- | --------------------------------------------------------------- |
| **author**                         | string   | The name of the validator (block producer).                     |
| **header**                         | object   | The main metadata header of the block.                          |
| **header.approvals**               | array    | An array of signatures encoded in standard `ed25519` format.    |
| **header.block_merkle_root**       | string   | Merkle root of the block.                                       |
| **header.block_ordinal**           | integer  | The ordinal number of the block.                                |
| **header.challenges_result**       | array    | List of challenge outcomes.                                     |
| **header.challenges_root**         | string   | Root of the challenges incurred in this block.                  |
| **header.chunk_headers_root**      | string   | Root of the chunk headers trie.                                 |
| **header.chunk_mask**              | boolean  | Boolean mask indicating included chunks.                        |
| **header.chunk_receipts_root**     | string   | Root of the receipts trie of the block.                         |
| **header.chunk_tx_root**           | string   | Root of the transaction trie of the block.                      |
| **header.chunks_included**         | integer  | Number of chunks included in the queried block.                 |
| **header.epoch_id**                | string   | Identifier of the epoch in which this block was produced.       |
| **header.epoch_sync_data_hash**    | string   | Epoch synchronization data hash.                                |
| **header.gas_price**               | string   | Gas price used for each transaction in the block (hexadecimal). |
| **header.hash**                    | string   | The block hash (unique identifier).                             |
| **header.height**                  | integer  | Block height (index in the blockchain).                         |
| **header.last_ds_final_block**     | string   | Hash of the last double-signature final block.                  |
| **header.last_final_block**        | string   | Hash of the last finalized block.                               |
| **header.latest_protocol_version** | integer  | Version number of the latest NEAR protocol.                     |
| **header.next_bp_hash**            | string   | Hash of the next block producer node.                           |
| **header.next_epoch_id**           | string   | Identifier of the next epoch.                                   |
| **header.outcome_root**            | string   | Root hash of the outcome trie.                                  |
| **header.prev_block_hash**         | string   | Hash of the previous block.                                     |
| **header.prev_height**             | integer  | Height of the previous block.                                   |
| **header.prev_state_root**         | string   | Root of the previous block state.                               |
| **header.random_value**            | string   | Random value generated to ensure deterministic outcomes.        |
| **header.rent_paid**               | string   | Number of tokens paid as rent.                                  |
| **header.signature**               | string   | `ed25519` digital signature of the block producer.              |
| **header.timestamp**               | integer  | Unix timestamp when the block was produced.                     |
| **header.timestamp_nanosec**       | string   | Timestamp in nanoseconds when the block was collated.           |
| **header.total_supply**            | string   | Total number of tokens in circulation.                          |
| **header.validator_proposals**     | array    | List of validator proposals for the next epoch.                 |
| **header.validator_reward**        | string   | Reward amount sent to validators.                               |
| **chunks**                         | array    | List of chunks included in the block.                           |
| **chunks.balance_burnt**           | string   | Number of tokens burnt in this chunk.                           |
| **chunks.chunk_hash**              | string   | Unique hash of the chunk.                                       |
| **chunks.encoded_length**          | integer  | Length of the chunk (encoded as integer).                       |
| **chunks.encoded_merkle_root**     | string   | Encoded Merkle root of the chunk (hexadecimal).                 |
| **chunks.gas_limit**               | string   | Maximum gas allowed for transactions in this chunk.             |
| **chunks.gas_used**                | string   | Total gas used by all transactions in this chunk.               |
| **chunks.height_created**          | integer  | Block height when this chunk was created.                       |
| **chunks.height_included**         | integer  | Block height where this chunk was included.                     |
| **chunks.outcome_root**            | string   | Outcome root hash of the chunk.                                 |
| **chunks.outgoing_receipts_root**  | string   | Root of the outgoing receipts of this chunk.                    |
| **chunks.prev_block_hash**         | string   | Previous block hash corresponding to this chunk.                |
| **chunks.prev_state_root**         | string   | Previous state root of the chunk.                               |
| **chunks.rent_paid**               | string   | Tokens paid as rent within the chunk.                           |
| **chunks.shard_id**                | integer  | ID of the shard where this chunk belongs.                       |
| **chunks.tx_root**                 | string   | Root of the transaction trie within this chunk.                 |
| **chunks.validator_proposals**     | array    | Validator proposals associated with this chunk.                 |
| **chunks.validator_reward**        | string   | Validator reward earned for processing the chunk.               |

## Use Cases
The `block` method can be used for:
  - Track recently produced blocks and validators.
  - Verify inclusion of transactions in a specific block.
  - Monitor gas price and network performance.
  - Retrieve block metadata for analytics or dashboards.

## Code Example
  - **Node(Axios)**
```js
     import axios from "axios";
    let data = JSON.stringify({
      "jsonrpc": "2.0",
      "method": "block",
      "params": {
        "finality": "final"
      },
      "id": "17821130"
    });
    
    let config = {
      method: 'post',
      maxBodyLength: Infinity,
      url: 'https://go.getblock.io/<ACCESS_TOKEN>',
      headers: { 
        'Content-Type': 'application/json'
      },
      data : data
    };
    
    axios.request(config)
    .then((response) => {
      console.log(JSON.stringify(response.data));
    })
    .catch((error) => {
      console.log(error);
    });
```
  - **Python(Requests)**
 ```python
    import requests
    import json
    
    url = "https://go.getblock.io/<ACCESS_TOKEN>"
    
    payload = json.dumps({
      "jsonrpc": "2.0",
      "method": "block",
      "params": {
        "finality": "final"
      },
      "id": "17821130"
    })
    headers = {
      'Content-Type': 'application/json'
    }
    
    response = requests.request("POST", url, headers=headers, data=payload)
    
    print(response.text)
```
## Error handling
| HTTP Code                     | Error Message               | Description                                            |
| ----------------------------- | --------------------------- | ------------------------------------------------------ |
| **400 Bad Request**           | `REQUEST_VALIDATION_ERROR`  | Failed parsing args: invalid type: null, expected string or map or expected one of `optimistic`, `near-final`, `final`.        |
| **500** | ` Internal Server Error`                | Network issues.              |

## Integration with Web3

By integrating the `block` method into your NEAR Core API setup, developers can:

- Fetch live blockchain data for block explorers.
- Power validator dashboards with real-time metrics.
- Support analytics tools to visualise transaction throughput.
