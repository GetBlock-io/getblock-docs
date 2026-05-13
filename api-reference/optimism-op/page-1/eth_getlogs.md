# eth\_getLogs

{% hint style="success" %}
The **eth\_getLogs** method returns event logs that match specified filter criteria on Optimism.
{% endhint %}

This method retrieves logs (events) emitted by smart contracts based on filter criteria including address, topics, and block range. It's essential for tracking contract events like token transfers, approvals, and other state changes. Large queries may be limited, so use specific filters and reasonable block ranges.

## Supported Networks

The eth\_getLogs **RPC Optimism** method supports the following network types:

* **Mainnet**
* **Testnet**: Sepolia

## Parameters

{% hint style="info" %}
One parameter is required:
{% endhint %}

| Parameter    | Type   | Description                                                                              | Required |
| ------------ | ------ | ---------------------------------------------------------------------------------------- | -------- |
| filterObject | Object | Filter object with fromBlock, toBlock, address, topics, and blockhash (optional) fields. | Yes      |

## Request

### URL

```
https://go.getblock.io/<ACCESS-TOKEN>/
```

To make a request using the eth\_getLogs **RPC Optimism** method via JSON-RPC, use the following **curl** command:

{% tabs %}
{% tab title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getLogs",
    "params": [{"fromBlock": "0x7A69B00", "toBlock": "0x7A69B2C", "address": "0x4200000000000000000000000000000000000006", "topics": ["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"]}],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="wss" %}
```bash
wscat -c wss://go.getblock.io/<ACCESS-TOKEN>/

# wait for connection and send the request body 
{"jsonrpc": "2.0", "method": "eth_getLogs", "params": [{"fromBlock": "0x7A69B00", "toBlock": "0x7A69B2C", "address": "0x4200000000000000000000000000000000000006", "topics": ["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"]}], "id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

## Response

A successful response returns the following:

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": [
        {
            "address": "0x4200000000000000000000000000000000000006",
            "topics": ["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef", "0x000000000000000000000000742d35cc6634c0532925a3b844bc9e7595f7bd5e"],
            "data": "0x0000000000000000000000000000000000000000000000000de0b6b3a7640000",
            "blockNumber": "0x7A69B0A",
            "transactionHash": "0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b",
            "transactionIndex": "0x0",
            "blockHash": "0x1d59ff54b1eb26b013ce3cb5fc9dab3705b415a67127a003c3e61eb445bb8df2",
            "logIndex": "0x0",
            "removed": false
        }
    ]
}
```

## Response Parameters

* **`id`**: A unique request identifier, matching the `id` sent in the request body.
* **`jsonrpc`**: Specifies the use of JSON-RPC version 2.0.
* **`result`**: An array of log objects, each containing address, topics, data, blockNumber, transactionHash, transactionIndex, blockHash, logIndex, and removed status.

## Use Case

The eth\_getLogs method is commonly used for:

* **Event monitoring**
* **Token transfer tracking**
* **Contract activity analysis**
* **DApp event handling**
* **Historical data retrieval**

## Code Example

Here's how to use the eth\_getLogs method with different programming languages:

{% tabs %}
{% tab title="Python" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"
headers = {
    "Content-Type": "application/json"
}
payload = {
    "jsonrpc": "2.0",
    "method": "eth_getLogs",
    "params": [{"fromBlock": "0x7A69B00", "toBlock": "0x7A69B2C", "address": "0x4200000000000000000000000000000000000006", "topics": ["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"]}],
    "id": "getblock.io"
}

response = requests.post(url, headers=headers, data=json.dumps(payload))

if response.status_code == 200:
    result = response.json().get("result")
    print("eth_getLogs result:", result)
else:
    print("Error:", response.status_code, response.text)
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

const url = "https://go.getblock.io/<ACCESS-TOKEN>/";
const headers = { "Content-Type": "application/json" };

const payload = {
    jsonrpc: "2.0",
    method: "eth_getLogs",
    params: [{"fromBlock": "0x7A69B00", "toBlock": "0x7A69B2C", "address": "0x4200000000000000000000000000000000000006", "topics": ["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"]}],
    id: "getblock.io"
};

axios.post(url, payload, { headers })
    .then(response => {
        if (response.status === 200) {
            console.log("eth_getLogs result:", response.data.result);
        } else {
            console.error("Error:", response.status, response.statusText);
        }
    })
    .catch(error => {
        console.error("Error:", error.response ? error.response.data : error.message);
    });
```
{% endtab %}
{% endtabs %}

If an error occurs, it could indicate connection issues, invalid parameters, or network problems. Always implement proper error handling in your applications.
