---
description: >-
  Example code for the eth_feeHistory json-rpc method. Сomplete guide on how to
  use eth_feeHistory json-rpc in GetBlock.io Web3 documentation.
---

# eth\_feeHistory - Binance Smart Chain

#### Parameters

`DATA, 8 Bytes` - None

Number of blocks in the requested range.

`DATA, 32 Bytes` - None

Number of newest block in the requested range.

`DATA, 32 Bytes` - None

Optional monotonically increasing list of percentile values to sample from each block's effective priority fees per gas in ascending order, weighted by gas used.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/v1/mainnet/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "eth_feeHistory",
"params": [61, "latest", [20, 60, 73]],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "baseFeePerGas": [
            "0x0",
            "0x0"
        ],
        "gasUsedRatio": [
            0.10737980714285714
        ],
        "oldestBlock": "0x1b5a217",
        "reward": [
            [
                "0xb2d05e00",
                "0xb2d05e00",
                "0xb2d05e00"
            ]
        ]
    }
}
```
