---
description: >-
  Retrieve logs for a specified filter using eth_getFilterLogs. Essential for
  tracking block events, transaction logs, or smart contract interactions in
  real-time within Ethereum dApps.
---

# eth\_getFilterLogs - Ethereum

{% hint style="success" %}
The eth\_getFilterLogs method is a JSON-RPC Ethereum API call that returns an array of logs for the specified filter
{% endhint %}

This eth\_getFilterLogs RPC Ethereum method is crucial for developers seeking to track block events, transaction logs, or smart contract interactions in real-time. To enhance log retrieval performance, it’s recommended to leave the --auto-log-bloom-caching-enabled option set to the default value of true.

eth\_getFilterLogs is a core API endpoint in Ethereum development, providing an easy way to retrieve logs based on filters created by previous methods like eth\_newFilter, eth\_newPendingTransactionFilter, or eth\_newBlockFilter. If a filter is invalid or expired, eth\_getFilterLogs error responses are returned.

### Supported Networks

* Filter ID (DATA):

A unique identifier for the filter whose logs are being requested. This Filter ID must be obtained from prior methods like eth\_newFilter, eth\_newPendingTransactionFilter, or eth\_newBlockFilter. The eth\_getlogs limit can be set to manage the number of logs returned, which is useful when filtering large amounts of data.

### Request

URL (API Endpoint)

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

{% tabs %}
{% tab title="curl" %}
```json
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \  
--header 'Content-Type: application/json' \  
--data-raw '{  
    "jsonrpc": "2.0",  
    "method": "eth_getFilterLogs",  
    "params": [  
        "0x5ace5de3985749b6a1b2b0d3f3e1fb69"  
    ],  
    "id": "getblock.io"  
}'  
```
{% endtab %}

{% tab title="ws" %}
```json
wscat -c wss://eth.getblock.io/YOUR-API-KEY/ 
# wait for connection and send the request body 
{"jsonrpc": "2.0",
"method": "eth_getFilterLogs",
"params": ["0x5ace5de3985749b6a1b2b0d3f3e1fb69"],
"id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

eth\_getFilterLogs example: In this example, a JSON-RPC request is made to fetch the logs related to the specified filter ID.

### Response

If the filter is not found, the response will include an error

```json
{  
    "error": {  
        "code": -32000,  
        "message": "filter not found"  
    },  
    "id": "getblock.io",  
    "jsonrpc": "2.0"  
} 
```

If logs are available for the filter, they will be returned as an array of log entries, which can be processed to monitor block or transaction events.

### Use Case

Tracking Contract Events:The Web3 eth\_getFilterLogs method allows developers to monitor smart contract events. By filtering for specific contract logs, you can track interactions with your decentralized application (dApp) in real time.

Monitoring Block Events:This method is essential for applications that need to monitor block data or events on the Ethereum blockchain, such as when new blocks are mined or specific state changes occur.

Transaction Logs:Developers can use Ethereum eth\_getFilterLogs to collect logs associated with transactions, including transaction receipts and their associated data.

### Code Example

Here’s how you can implement the eth\_getFilterLogs method using different programming languages

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
    "method": "eth_getFilterLogs",
    "params": [
        "0x5ace5de3985749b6a1b2b0d3f3e1fb69"
    ],
    "id": "getblock.io"
}

response = requests.post(url, headers=headers, data=json.dumps(payload))

if response.status_code == 200:
    print(response.json())
else:
    print(f"Ошибка: {response.status_code}", response.text)
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

// Replace <ACCESS-TOKEN> with your GetBlock API key
const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

// Request payload
const requestBody = {
  jsonrpc: '2.0',
  method: 'eth_getFilterLogs',
  params: ['0x5ace5de3985749b6a1b2b0d3f3e1fb69'], // Filter ID
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
    console.log('Filter Logs Response:', response.data.result);
  })
  .catch((error) => {
    console.error('Error fetching filter logs:', error.message);
  });
```
{% endtab %}
{% endtabs %}

Setup:Replace \<ACCESS-TOKEN> with your GetBlock API key.Use FILTER\_ID from a prior filter creation method (e.g., eth\_newFilter).

Request:Sends a POST request to the JSON-RPC endpoint, calling the eth\_getFilterLogs method and passing the FILTER\_ID as a parameter.

Response Handling:Logs any errors, such as the eth\_getFilterLogs error like "filter not found," if the filter is expired or invalid.Outputs an array of log entries if successful.

Usage:The eth\_getFilterLogs method is part of the Core API for Ethereum, enabling developers to efficiently query logs for specific filters. This method is especially helpful for applications that need to track changes to block or transaction data in real-time.
