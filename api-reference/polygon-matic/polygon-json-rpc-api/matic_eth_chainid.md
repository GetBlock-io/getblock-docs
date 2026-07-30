---
description: >-
  Example code for the eth_chainId json-rpc method. Сomplete guide on how to use
  eth_chainId json-rpc in GetBlock.io Web3 documentation.
---

# eth\_chainId - Polygon

The `eth_chainId` method returns the current chain ID used for signing replay-protected transactions (EIP-155). This method is essential for applications that need to verify they are connected to the correct network and for properly signing transactions.

## Parameters

* None

## Request

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_chainId",
    "params": [],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
    jsonrpc: '2.0',
    method: 'eth_chainId',
    params: [],
    id: 'getblock.io'
};

axios.post(url, payload, {
    headers: { 'Content-Type': 'application/json' }
})
.then(response => {
    const chainId = parseInt(response.data.result, 16);
    console.log('Chain ID:', chainId);
})
.catch(error => console.error(error));
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="Python (requests)" %}
```python
import requests

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "method": "eth_chainId",
    "params": [],
    "id": "getblock.io"
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
result = response.json()
chain_id = int(result["result"], 16)
print(f"Chain ID: {chain_id}")
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="Rust (reqwest)" %}
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
            "method": "eth_chainId",
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

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x89"
}
```

## Response Parameters

| Field   | Type   | Description                                                     |
| ------- | ------ | --------------------------------------------------------------- |
| jsonrpc | string | JSON-RPC version (2.0)                                          |
| id      | string | Request identifier                                              |
| result  | string | Chain ID in hexadecimal format (0x89 = 137 for Polygon Mainnet) |

## Use Case

The `eth_chainId` method is essential for:

* Network verification: Confirm connection to the correct network before operations
* Transaction signing: Include correct chain ID for EIP-155 replay protection
* Multi-chain applications: Dynamically detect and switch between networks
* Wallet integration: Verify network configuration matches user expectations
* Security: Prevent cross-chain replay attacks

### Example: Network Verification

{% code title="JavaScript (example)" %}
```javascript
const POLYGON_MAINNET_CHAIN_ID = 137;
const POLYGON_AMOY_CHAIN_ID = 80002;

async function verifyNetwork(provider) {
    const chainId = await provider.send('eth_chainId', []);
    const numericChainId = parseInt(chainId, 16);
    
    if (numericChainId === POLYGON_MAINNET_CHAIN_ID) {
        console.log('Connected to Polygon Mainnet');
        return 'mainnet';
    } else if (numericChainId === POLYGON_AMOY_CHAIN_ID) {
        console.log('Connected to Polygon Amoy Testnet');
        return 'testnet';
    } else {
        throw new Error(`Unexpected chain ID: ${numericChainId}`);
    }
}
```
{% endcode %}

## Error Handling

| Status Code | Error Message   | Cause                           |
| ----------- | --------------- | ------------------------------- |
| 403         | Forbidden       | Missing or invalid ACCESS-TOKEN |
| -32600      | Invalid Request | Malformed request body          |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="Ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

// Using network property
const network = await provider.getNetwork();
console.log('Chain ID:', network.chainId);

// Using raw RPC call
const chainIdHex = await provider.send('eth_chainId', []);
console.log('Chain ID (hex):', chainIdHex);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="Viem" %}
```javascript
import { createPublicClient, http } from 'viem';
import { polygon } from 'viem/chains';

const client = createPublicClient({
    chain: polygon,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const chainId = await client.getChainId();
console.log('Chain ID:', chainId);
```
{% endcode %}
{% endtab %}

{% tab title="Web3.js" %}
{% code title="Web3.js" %}
```javascript
import Web3 from 'web3';

const web3 = new Web3('https://go.getblock.io/<ACCESS-TOKEN>/');

const chainId = await web3.eth.getChainId();
console.log('Chain ID:', chainId);
```
{% endcode %}
{% endtab %}
{% endtabs %}
