# eth\_getBlockByNumber

{% hint style="success" %}
The **eth\_getBlockByNumber** method returns detailed information about a specific block on Optimism identified by its number.
{% endhint %}

This method retrieves comprehensive block data including the block hash, parent hash, miner address, state root, transactions, gas used, and timestamps. You can request either full transaction objects or just transaction hashes. You can also use block tags like `'latest'`, `'earliest'`, or `'pending'`. This is fundamental for block explorers, analytics tools, and applications that need to process block data.

### Supported Networks

The eth\_getBlockByNumber **RPC Optimism** method supports the following network types:

* **Mainnet**
* **Testnet**: Sepolia

### Parameters

{% hint style="info" %}
Two parameters are required:
{% endhint %}

| Parameter        | Type          | Description                                                                           | Required |
| ---------------- | ------------- | ------------------------------------------------------------------------------------- | -------- |
| blockNumber      | QUANTITY\|TAG | Block number in hex, or `'latest'`, `'earliest'`, `'pending'`.                        | Yes      |
| fullTransactions | Boolean       | If true, returns full transaction objects; if false, returns only transaction hashes. | Yes      |

### Request

#### URL

```
https://go.getblock.io/<ACCESS-TOKEN>/
```

To make a request using the eth\_getBlockByNumber **RPC Optimism** method via JSON-RPC, use the following **curl** command:

{% tabs %}
{% tab title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getBlockByNumber",
    "params": ["latest", false],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="wss" %}
```bash
wscat -c wss://go.getblock.io/<ACCESS-TOKEN>/

# wait for connection and send the request body 
{"jsonrpc": "2.0", "method": "eth_getBlockByNumber", "params": ["latest", false], "id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

### Response

A successful response returns the following:

```json
{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": {
    "number": "0x7A69B2C",
    "hash": "0x1d59ff54b1eb26b013ce3cb5fc9dab3705b415a67127a003c3e61eb445bb8df2",
    "parentHash": "0x9b83c12c69edb74f6c8dd5d052765c1adf940e320bd1291696e6fa07829eee71",
    "timestamp": "0x6571f8c0",
    "gasLimit": "0x1c9c380",
    "gasUsed": "0x5208",
    "transactions": ["0xabc123...", "0xdef456..."]
  }
}
```

### Response Parameters

* **`id`**: A unique request identifier, matching the `id` sent in the request body.
* **`jsonrpc`**: Specifies the use of JSON-RPC version 2.0.
* **`result`**: A block object containing block details including number, hash, parentHash, timestamp, gasLimit, gasUsed, and transactions array.

### Use Case

The eth\_getBlockByNumber method is commonly used for:

* **Block exploration**
* **Transaction history**
* **Chain analysis**
* **Data indexing**

### Code Example

Here's how to use the eth\_getBlockByNumber method with different programming languages:

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
    "method": "eth_getBlockByNumber",
    "params": ["latest", false],
    "id": "getblock.io"
}

response = requests.post(url, headers=headers, data=json.dumps(payload))

if response.status_code == 200:
    result = response.json().get("result")
    print("eth_getBlockByNumber result:", result)
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
    method: "eth_getBlockByNumber",
    params: ["latest", false],
    id: "getblock.io"
};

axios.post(url, payload, { headers })
    .then(response => {
        if (response.status === 200) {
            console.log("eth_getBlockByNumber result:", response.data.result);
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
