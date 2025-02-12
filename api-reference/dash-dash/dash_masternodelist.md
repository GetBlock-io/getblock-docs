---
description: >-
  Example code for the masternodelist json-rpc method. Сomplete guide on how to
  use masternodelist json-rpc in GetBlock.io Web3 documentation.
---

# masternodelist - Dash

#### Parameters

`mode` - string

Optional. Required to use filter.

The mode to run list in.

addr: Print IP address associated with a masternode (can be additionally filtered, partial match)

full: Print info in format "status payee lastpaidtime lastpaidblock IP" (can be additionally filtered, partial match)

info: Print info in format "status payee IP" (can be additionally filtered, partial match)

json (Default): Print info in JSON format (can be additionally filtered, partial match)

lastpaidblock: Print the last block height a node was paid on the network

lastpaidtime: Print the last time a node was paid on the network

owneraddress: Print the masternode owner Dash address

payee: Print Dash address associated with a masternode (can be additionally filtered, partial match)

publicKeyOperator: Print the masternode operator public key

status: Print masternode status: ENABLED / POSE\_BANNED (can be additionally filtered, partial match)

votingaddress: Print the masternode voting Dash address

`filter` - string

Optional

Filter results. Partial match by outpoint by default in all modes, additional matches in some modes are also available.

#### Request

```java
curl --location --request POST 'https://dash.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "masternodelist",
"params": [null, null],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": {
        "000be8f5f64b7bf26029218f647af1ff16a25c0fc1747930a5ee4e03860ec581-1": {
            "address": "45.76.234.147:9999",
            "collateraladdress": "Xsyw4nDP682YTgg7Bp426pGC3FhHZyMCna",
            "lastpaidblock": 1532684,
            "lastpaidtime": 1630758460,
            "owneraddress": "Xqqoaw8MpJ4BN9sqvZbr893HDhBpMaAe57",
            "payee": "XiwYx9vPDsviVbARe9deBs9QFsgYZNAHrC",
            "proTxHash": "abdfdd443daee7fa10d2e635ec03e34b1fff57db847855fc80e08da9ae84df88",
            "pubkeyoperator": "860413b84c02b5bfd97f44a2737dc4bd20404614d74e63da02d3dd91fd211d7c5b4ffc9caa23b277b53b96ec50bd7ff7",
            "status": "ENABLED",
            "votingaddress": "Xqqoaw8MpJ4BN9sqvZbr893HDhBpMaAe57"
        },
        "0013a0c62b7ab3857ac406f14cc266bd3acaa76e777bc36c2773405f02a2e8a6-0": {
            "address": "135.181.82.13:9999",
            "collateraladdress": "XsyQ1DLmBA28cMsadzmAMvd2DbtrHnsBQG",
            "lastpaidblock": 1533229,
            "lastpaidtime": 1630844680,
            "owneraddress": "Xy8rGip25xwHUrgme1W4o59W2k5KJz4bsG",
            "payee": "XeMARmdS6jidwhCTdvT6Yg1Fw2CmQsNMLS",
            "proTxHash": "6818ac51bb67ec8214733559b6f196f3777a9e07a7cab72330389ab424f6b03b",
            "pubkeyoperator": "8689c483c34a29ec487b9f057408ceb2b6d00f752a97a466c8286a85fb385d8b99abbed30aa5353327d63af5f8ea5b15",
            "status": "ENABLED",
            "votingaddress": "Xy8rGip25xwHUrgme1W4o59W2k5KJz4bsG"
        }
    }
}
```
