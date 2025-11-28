---
description: >-
  Poll a specific filter for changes since the last check using
  eth_getFilterChanges. Vital for real-time monitoring of logs, transactions,
  and blocks in dApps and Ethereum applications.
---

# eth\_getFilterChanges - Ethereum

{% hint style="success" %}
The eth\_getFilterChanges method is part of the JSON-RPC Ethereum API, designed to poll a specific filter and retrieve an array of changes since the last poll
{% endhint %}

This core API endpoint is vital for tracking updates to logs, pending transactions, or new blocks in real-time. If the filter is invalid or expired, the method returns an eth\_getFilterChanges error.

As a critical feature in Ethereum eth\_getFilterChanges, this method is commonly used in decentralized applications (dApps) to monitor and react to on-chain events efficiently.The eth\_getFilterChanges endpoints allows developers to integrate real-time blockchain monitoring into their applications seamlessly.

### Supported Networks

The eth\_getFilterChanges RPC Ethereum method is supported on the following network types

* Mainnet
* Testnet: Sepolia, Hoodi

### Parameters

Filter ID (DATA):

A unique identifier for the filter being polled. This identifier must be obtained through prior methods like eth\_newFilter, eth\_newPendingTransactionFilter, or eth\_newBlockFilter.

### Request Example

URL (API Endpoint)

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

Here is an eth\_getFilterChanges example of how to use the method in a JSON-RPC request

{% tabs %}
{% tab title="curl" %}
```json
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getFilterChanges",
    "params": [
        "0xf8bf5598d9e04fbe84523d42640b9b0e"
    ],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="wss" %}
```json
wscat -c wss://go.getblock.io/<ACCESS-TOKEN>/
# wait for connection and send the request body 
{"jsonrpc": "2.0",
"method": "eth_getFilterChanges",
"params": ["0xf8bf5598d9e04fbe84523d42640b9b0e"],
"id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

### Response Example

When the filter returns changes, they are provided as an array of data, such as logs or pending transactions

```json
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": [
        {
            "address": "0xa50a51c09a5c451c52bb714527e1974b686d8e77",
            "topics": [
                "0x5c2c447c5d5caff32446f5a9c1df2ffdf501b43b3f4e1b9f989d2d17d17e489e"
            ],
            "data": "0x",
            "blockNumber": "0x5b8d80",
            "transactionHash": "0xb16d2cfc99a453b09c8e6b61b582f1b6ac14b36f7e8fbe5433e7a1a1fd3e3c34",
            "transactionIndex": "0x1",
            "blockHash": "0x72a097d3c95f2ef340c29e2da5f14a5fd5078c741c54b6f372d7e455f65a26f3",
            "logIndex": "0x0",
            "removed": false
        }
    ]
}
```

If the filter is not found or has expired

```
{
    "error": {
        "code": -32000,
        "message": "filter not found"
    },
    "id": "getblock.io",
    "jsonrpc": "2.0"
}
```

### Use Case

Tracking Log Events:\
The eth\_getFilterChanges API method enables developers to monitor log updates for specific smart contracts. This is particularly valuable for applications that need to stay updated on events triggered by smart contracts.

Monitoring Pending Transactions:\
By polling a pending transaction filter, you can track newly added transactions to the mempool in real-time, improving insights into transaction flow.

Listening for New Blocks:\
Using block filters with eth\_getFilterChanges RPC Ethereum, developers can identify new blocks as they are mined, ensuring accurate synchronization with the blockchain.

### Code Example

Here’s how you can implement eth\_getFilterChanges using different programming languages

{% tabs %}
{% tab title="Python" %}
```python
import requests
import json

url = 'https://go.getblock.io/<ACCESS-TOKEN>/'
headers = {
    'Content-Type': 'application/json'
}

payload = {
    "jsonrpc": "2.0",
    "method": "eth_getFilterChanges",
    "params": [
        "0xf8bf5598d9e04fbe84523d42640b9b0e"
    ],
    "id": "getblock.io"
}

response = requests.post(url, headers=headers, data=json.dumps(payload))

if response.status_code == 200:
    print(response.json())
else:
    print(f"Error: {response.status_code}", response.text)

```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

// Replace <ACCESS-TOKEN> with your actual API key
const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

// Request payload
const requestBody = {
  jsonrpc: '2.0',
  method: 'eth_getFilterChanges',
  params: ['0xf8bf5598d9e04fbe84523d42640b9b0e'], // Filter ID
  id: 'getblock.io',
};

// Axios POST request
axios
  .post(url, requestBody, {
    headers: {
      'Content-Type': 'application/json',
    },
  })
  .then((response) => {
    console.log('Filter Changes Response:', response.data.result);
  })
  .catch((error) => {
    console.error('Error fetching filter changes:', error.message);
  });
```
{% endtab %}
{% endtabs %}

This eth\_getFilterChanges example demonstrates how to use the method :

Setup:Replace \<ACCESS-TOKEN> with your GetBlock API key.Use FILTER\_ID from a filter creation method such as eth\_newFilter.

Request:The method sends a POST request to the JSON RPC endpoint, specifying the eth\_getFilterChanges method and the filter ID as parameters.

Response Handling:If no changes occur, the method returns an empty array.Logs an error, such as "filter not found", if the filter has expired or is invalid.

Value Handling: The value of the filter changes is extracted from the response and logged.

Usage:The Web3 eth\_getFilterChanges method is essential for applications requiring real-time updates on Ethereum network activity, including logs, transactions, and new blocks.

\\
