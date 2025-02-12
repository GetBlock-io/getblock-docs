---
description: >-
  Example code for the masternode_list json-rpc method. Сomplete guide on how to
  use masternode_list json-rpc in GetBlock.io Web3 documentation.
---

# masternode\_list - Dash

#### Parameters

`method name` - string

None

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
"method": "masternode",
"params": ["list", "full", "ENABLED"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": {
        "000be8f5f64b7bf26029218f647af1ff16a25c0fc1747930a5ee4e03860ec581-1": "           ENABLED XiwYx9vPDsviVbARe9deBs9QFsgYZNAHrC 1630758460 1532684 45.76.234.147:9999",
        "0013a0c62b7ab3857ac406f14cc266bd3acaa76e777bc36c2773405f02a2e8a6-0": "           ENABLED XeMARmdS6jidwhCTdvT6Yg1Fw2CmQsNMLS 1630844680 1533229 135.181.82.13:9999",
        "0071bb3c7029bec401a9c548be5c1ea6b4ac24dccd3c602d2641d50432f0e467-1": "           ENABLED Xs9JSHbRxHwfVDuDrQnyzMYVGNWuHWLoit 1630677091 1532173 212.24.104.235:9999"
    }
}
```
