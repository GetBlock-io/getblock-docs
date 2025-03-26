---
description: >-
  Example code for the eth_getChainConfig json-rpc method. Сomplete guide on how
  to use eth_getChainConfig json-rpc in GetBlock.io Web3 documentation.
---

# eth\_getChainConfig - Avalanche

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' 
--header 'Content-Type: application/json' 
--data-raw '{
  "jsonrpc": "2.0",
  "method": "eth_getChainConfig",
  "params": [],
  "id": "getblock.io"
}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "apricotPhase1BlockTimestamp": 1617199200,
        "apricotPhase2BlockTimestamp": 1620644400,
        "apricotPhase3BlockTimestamp": 1629813600,
        "apricotPhase4BlockTimestamp": 1632344400,
        "apricotPhase5BlockTimestamp": 1638468000,
        "apricotPhase6BlockTimestamp": 1662494400,
        "apricotPhasePost6BlockTimestamp": 1662519600,
        "apricotPhasePre6BlockTimestamp": 1662341400,
        "banffBlockTimestamp": 1666108800,
        "byzantiumBlock": 0,
        "chainId": 43114,
        "constantinopleBlock": 0,
        "cortinaBlockTimestamp": 1682434800,
        "daoForkBlock": 0,
        "daoForkSupport": true,
        "eip150Block": 0,
        "eip150Hash": "0x2086799aeebeae135c246c65021c82b4e15a2c451340993aacfd2751886514f0",
        "eip155Block": 0,
        "eip158Block": 0,
        "homesteadBlock": 0,
        "istanbulBlock": 0,
        "muirGlacierBlock": 0,
        "petersburgBlock": 0
    }
}
```
