---
description: >-
  Example code for the eth_blockNumber json-rpc method. Сomplete guide on how to
  use eth_blockNumber json-rpc in GetBlock.io Web3 documentation.
---

# eth\_blockNumber - Polygon

The **eth\_blockNumber** method returns the number of the most recent block on the Polygon blockchain. This method provides real-time insight into the blockchain's progress and is essential for applications that need to stay synchronized with the latest state of the network.

## Parameters

* None

## Request

{% tabs %}
{% tab title="First Tab" %}
{% code title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_blockNumber",
    "params": [],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Websocket" %}
{% code title="wscat" %}
```bash
wscat -c wss://go.getblock.io/<ACCESS-TOKEN>/
# wait for connection and send the request body 
{"jsonrpc": "2.0", "method": "eth_blockNumber", "params": [], "id": "getblock.io"}
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="axios.js" %}
```javascript
import axios from 'axios';

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
    jsonrpc: '2.0',
    method: 'eth_blockNumber',
    params: [],
    id: 'getblock.io'
};

axios.post(url, payload, {
    headers: { 'Content-Type': 'application/json' }
})
.then(response => {
    const blockNumber = parseInt(response.data.result, 16);
    console.log('Current Block Number:', blockNumber);
})
.catch(error => console.error(error));
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="requests.py" overflow="wrap" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "method": "eth_blockNumber",
    "params": [],
    "id": "getblock.io"
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)

if response.status_code == 200:
    result = response.json()
    block_number = int(result["result"], 16)
    print(f"Current Block Number: {block_number}")
else:
    print(f"Error: {response.status_code}")
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="main.rs" %}
```rust
use reqwest::header;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = reqwest::Client::new();
    
    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header(header::CONTENT_TYPE, "application/json")
        .body(r#"{
            "jsonrpc": "2.0",
            "method": "eth_blockNumber",
            "params": [],
            "id": "getblock.io"
        }"#)
        .send()
        .await?;
    
    println!("{}", response.text().await?);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

{% code title="response.json" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x3A2B5C7"
}
```
{% endcode %}

## Response Parameters

| Field   | Type   | Description                                                                                        |
| ------- | ------ | -------------------------------------------------------------------------------------------------- |
| jsonrpc | string | JSON-RPC version (2.0)                                                                             |
| id      | string | Request identifier                                                                                 |
| result  | string | The latest block number in hexadecimal format. For example, "0x3A2B5C7" equals 61077959 in decimal |

## Use Case

The eth\_blockNumber method is essential for:

* **Block synchronization**: Verify if your application is synced with the network
* **Transaction confirmation**: Monitor block progress while waiting for transaction confirmations
* **Time-based triggers**: Execute actions when specific block heights are reached
* **Data indexing**: Track the latest block for event scanning and data extraction
* **Health monitoring**: Check if the node is producing new blocks

### Example: Block Confirmation Monitor

{% code title="confirm_monitor.py" %}
```python
import requests
import time

def wait_for_confirmations(target_block, confirmations=12):
    url = "https://go.getblock.io/<ACCESS-TOKEN>/"
    payload = {"jsonrpc": "2.0", "method": "eth_blockNumber", "params": [], "id": 1}
    
    while True:
        response = requests.post(url, json=payload)
        current_block = int(response.json()["result"], 16)
        
        if current_block >= target_block + confirmations:
            print(f"Transaction confirmed at block {current_block}")
            break
        
        print(f"Waiting... Current: {current_block}, Target: {target_block + confirmations}")
        time.sleep(2)  # Polygon produces blocks every ~2 seconds
```
{% endcode %}

## Error Handling

| Status Code | Error Message   | Cause                           |
| ----------- | --------------- | ------------------------------- |
| 403         | Forbidden       | Missing or invalid ACCESS-TOKEN |
| -32600      | Invalid Request | Malformed request body          |
| -32603      | Internal error  | Node synchronization issues     |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

// Using provider method
const blockNumber = await provider.getBlockNumber();
console.log('Current Block Number:', blockNumber);

// Using raw RPC call
const hexBlockNumber = await provider.send('eth_blockNumber', []);
console.log('Block Number (hex):', hexBlockNumber);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { polygon } from 'viem/chains';

const client = createPublicClient({
    chain: polygon,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const blockNumber = await client.getBlockNumber();
console.log('Current Block Number:', blockNumber);
```
{% endcode %}
{% endtab %}

{% tab title="Web3.js" %}
{% code title="web3.js" %}
```javascript
import Web3 from 'web3';

const web3 = new Web3('https://go.getblock.io/<ACCESS-TOKEN>/');

const blockNumber = await web3.eth.getBlockNumber();
console.log('Current Block Number:', blockNumber);
```
{% endcode %}
{% endtab %}
{% endtabs %}
