# eth\_call - Blast

This method executes a new message call immediately without creating a transaction on the blockchain. It is the primary method for reading data from smart contracts, including token balances, contract state, and view/pure function results.

## Parameters

| Parameter      | Type   | Required | Description                                             |
| -------------- | ------ | -------- | ------------------------------------------------------- |
| transaction    | object | Yes      | Transaction call object                                 |
| blockParameter | string | Yes      | Block number in hex, or "latest", "earliest", "pending" |

### Transaction Object

| Field    | Type   | Required | Description                        |
| -------- | ------ | -------- | ---------------------------------- |
| from     | string | No       | 20-byte sender address             |
| to       | string | Yes      | 20-byte recipient/contract address |
| gas      | string | No       | Gas limit for the call (hex)       |
| gasPrice | string | No       | Gas price in wei (hex)             |
| value    | string | No       | Value to send in wei (hex)         |
| data     | string | No       | Encoded function call data         |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_call",
    "params": [{ "to": "0x4200000000000000000000000000000000000006", "data": "0x70a08231000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa96045" }, "latest"],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'eth_call',
    params: [{ to: '0x4200000000000000000000000000000000000006', data: '0x70a08231000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa96045' }, 'latest'],
    id: 'getblock.io'
}, {
    headers: { 'Content-Type': 'application/json' }
});

console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests

response = requests.post(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'eth_call',
        'params': [{ 'to': '0x4200000000000000000000000000000000000006', 'data': '0x70a08231000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa96045' }, 'latest'],
        'id': 'getblock.io'
    }
)

print(response.json())
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="example.rs" %}
```rust
use reqwest::Client;
use serde_json::{json, Value};

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();

    let response = client
        .post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&json!({
            "jsonrpc": "2.0",
            "method": "eth_call",
            "params": [{ "to": "0x4200000000000000000000000000000000000006", "data": "0x70a08231000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa96045" }, "latest"],
            "id": "getblock.io"
        }))
        .send()
        .await?
        .json::<Value>()
        .await?;

    println!("Result: {}", response["result"]);
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
    "result": "0x0000000000000000000000000000000000000000000000000de0b6b3a7640000"
}
```

## Response Parameters

| Parameter | Type   | Description                                    |
| --------- | ------ | ---------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")              |
| id        | string | Request identifier matching the request        |
| result    | string | Hex-encoded return data from the contract call |

## Use Cases

* **Token Balances**: Query ERC-20 token balances using balanceOf
* **Contract State**: Read public state variables from smart contracts
* **Price Feeds**: Query oracle prices from Chainlink or other protocols
* **DeFi Calculations**: Query pool reserves, exchange rates, and liquidity
* **L1 Fee Estimation**: Call the OP Stack GasPriceOracle predeploy to preview L1 data fees

## Error Handling

| Error Code | Message            | Description                                   |
| ---------- | ------------------ | --------------------------------------------- |
| -32602     | Invalid params     | Invalid transaction object or block parameter |
| -32603     | Internal error     | Contract execution error                      |
| 3          | Execution reverted | The contract reverted the call                |
| -32000     | Execution error    | Insufficient gas or other execution failure   |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

const result = await provider.call({ to: '0x4200000000000000000000000000000000000006', data: '0x70a08231000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa96045' });
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" %}
```javascript
import { createPublicClient, http } from 'viem';

const client = createPublicClient({
  transport: http('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/')
});

const balance = await client.readContract({ address: '0x4200000000000000000000000000000000000006', abi: erc20Abi, functionName: 'balanceOf', args: ['0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045'] });
```
{% endcode %}
{% endtab %}
{% endtabs %}
