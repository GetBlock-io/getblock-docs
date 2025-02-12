---
description: >-
  Example code for the dag_getEvent json-rpc method. Сomplete guide on how to
  use dag_getEvent json-rpc in GetBlock.io Web3 documentation.
---

# dag\_getEvent - Fantom

#### Parameters

`DATA` - None

Hexadecimal event ID.

#### Request

```java
curl --location --request POST 'https://ftm.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "dag_getEvent",
"params": ["0x00033bbf00000719167a3b2c445bd3e53e9c35d16cf52ac24852317b9605d07e"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "anyBlockVotes": false,
        "anyEpochVote": false,
        "anyMisbehaviourProofs": false,
        "anyTxs": false,
        "creationTime": "0x17613e861ace703e",
        "creator": "0x53",
        "epoch": "0x33bbf",
        "extraData": "0x",
        "frame": "0x80",
        "gasPowerLeft": {
            "longTerm": "0x5cf963",
            "shortTerm": "0x36ce49"
        },
        "gasPowerUsed": "0x61a8",
        "id": "0x00033bbf00000719167a3b2c445bd3e53e9c35d16cf52ac24852317b9605d07e",
        "lamport": "0x719",
        "medianTime": "0x17613e85f0fce95b",
        "networkVersion": "0x0",
        "parents": [
            "0x00033bbf000007122dc6a3985d896190553a12ef7c632904abf0c356f781dbc2",
            "0x00033bbf000007175a4d2753b292401e2094204bea4bd884f205e46f79fbc504",
            "0x00033bbf00000716641d26c81fab6934f6b932c4efd4ca112eaeafe8c945d0e0",
            "0x00033bbf000007166b55a48d374e9b143c984f81e96ac48844371e606a314ef4",
            "0x00033bbf0000071834483fdc25c0d46f3b7e142084d1a563ad3cdd8679ff16c2"
        ],
        "payloadHash": "0x2fd8996df84d1ab44b758d0c383c2596b9c34d46a980a7a9f7a389020002fe7c",
        "prevEpochHash": null,
        "seq": "0x29",
        "version": "0x1"
    }
}
```
