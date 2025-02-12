---
description: >-
  Example code for the masternode_status json-rpc method. Сomplete guide on how
  to use masternode_status json-rpc in GetBlock.io Web3 documentation.
---

# masternode\_status - Dash

#### Parameters

`method name` - string

None

#### Request

```java
curl --location --request POST 'https://dash.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "masternode",
"params": ["status"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": {
        "outpoint": "d1be3a1aa0b9516d06ed180607c168724c21d8ccf6c5a3f5983769830724c357-0",
        "service": "45.32.237.76:19999",
        "proTxHash": "04d06d16b3eca2f104ef9749d0c1c17d183eb1b4fe3a16808fd70464f03bcd63",
        "collateralHash": "d1be3a1aa0b9516d06ed180607c168724c21d8ccf6c5a3f5983769830724c357",
        "collateralIndex": 0,
        "dmnState": {
            "service": "45.32.237.76:19999",
            "registeredHeight": 7402,
            "lastPaidHeight": 59721,
            "PoSePenalty": 0,
            "PoSeRevivedHeight": 61915,
            "PoSeBanHeight": -1,
            "revocationReason": 0,
            "ownerAddress": "yT8DDY5NkX4ZtBkUVz7y1RgzbakCnMPogh",
            "votingAddress": "yMLrhooXyJtpV3R2ncsxvkrh6wRennNPoG",
            "payoutAddress": "yTsGq4wV8WF5GKLaYV2C43zrkr2sfTtysT",
            "pubKeyOperator": "02a2e2673109a5e204f8a82baf628bb5f09a8dfc671859e84d2661cae03e6c6e198a037e968253e94cd099d07b98e94e"
        },
        "state": "READY",
        "status": "Ready"
    }
}
```
