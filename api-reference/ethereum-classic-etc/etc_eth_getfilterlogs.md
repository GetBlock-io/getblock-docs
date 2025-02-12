---
description: >-
  Example code for the eth_getFilterLogs json-rpc method. Сomplete guide on how
  to use eth_getFilterLogs json-rpc in GetBlock.io Web3 documentation.
---

# eth\_getFilterLogs - Ethereum Classic

#### Parameters

`data` - string

Filter ID.

#### Request

```java
curl --location --request POST 'https://etc.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getFilterLogs",
"params": ["0xf7d8be7ac9ce5723ada9c90cda19e5c8"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": [
        {
            "logIndex": "0x0",
            "removed": false,
            "blockNumber": "0xb3",
            "blockHash": "0xe7cd776bfee2fad031d9cc1c463ef947654a031750b56fed3d5732bee9c61998",
            "transactionHash": "0xff36c03c0fba8ac4204e4b975a6632c862a3f08aa01b004f570cc59679ed4689",
            "transactionIndex": "0x0",
            "address": "0x2e1f232a9439c3d459fceca0beef13acc8259dd8",
            "data": "0x0000000000000000000000000000000000000000000000000000000000000003",
            "topics": [
                "0x04474795f5b996ff80cb47c148d4c5ccdbe09ef27551820caa9c2f8ed149cce3"
            ]
        },
        {
            "logIndex": "0x0",
            "removed": false,
            "blockNumber": "0xb6",
            "blockHash": "0x3f4cf35e7ed2667b0ef458cf9e0acd00269a4bc394bb78ee07733d7d7dc87afc",
            "transactionHash": "0x117a31d0dbcd3e2b9180c40aca476586a648bc400aa2f6039afdd0feab474399",
            "transactionIndex": "0x0",
            "address": "0x2e1f232a9439c3d459fceca0beef13acc8259dd8",
            "data": "0x0000000000000000000000000000000000000000000000000000000000000005",
            "topics": [
                "0x04474795f5b996ff80cb47c148d4c5ccdbe09ef27551820caa9c2f8ed149cce3"
            ]
        }
    ]
}
```
