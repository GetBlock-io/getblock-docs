# eth\_newPendingTransactionFilter

This method creates a filter that can be polled to receive notifications about new pending transactions.

### Parameters

* None

### Request Sample

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_newPendingTransactionFilter",
    "params": [],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="JavaScript (Axios)" %}
```javascript
const axios = require('axios');

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
    jsonrpc: '2.0',
    method: 'eth_newPendingTransactionFilter',
    params: [],
    id: 'getblock.io'
};

axios.post(url, payload, {
    headers: { 'Content-Type': 'application/json' }
})
.then(response => console.log(response.data))
.catch(error => console.error(error));
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="Python" overflow="wrap" %}
```python
import requests

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "method": "eth_newPendingTransactionFilter",
    "params": [],
    "id": "getblock.io"
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
print(response.json())
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="Rust" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    
    let payload = json!({
        "jsonrpc": "2.0",
        "method": "eth_newPendingTransactionFilter",
        "params": [],
        "id": "getblock.io"
    });

    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&payload)
        .send()
        .await?;

    let result: serde_json::Value = response.json().await?;
    println!("{:#?}", result);
    
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

### Response

A successful response returns the following:

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x2871b63cab64878d61f16974fbf11c7d"
}

```

### Response Parameters

* **`id`**: A unique request identifier, matching the `id` sent in the request body.
* **`jsonrpc`**: Specifies the use of JSON-RPC version 2.0.
* **`result`**: A filter ID that can be used with `eth_getFilterChanges`.

### Use Case

The eth\_newPendingTransactionFilter method is commonly used for:

* **Mempool monitoring**
* **Transaction tracking**
* **Pending transaction analysis**

## Error Handling

| Error Code | Description          |
| ---------- | -------------------- |
| -32603     | Internal error       |
| -32601     | Method not supported |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="Ethers.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

provider.on('pending', (txHash) => {
    console.log('Pending tx:', txHash);
});
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="Viem" %}
```javascript
import { createPublicClient, http } from 'viem';
import { optimism } from 'viem/chains';

const client = createPublicClient({
    chain: optimism,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const unwatch = client.watchPendingTransactions({
    onTransactions: hashes => console.log(hashes)
});
```
{% endcode %}
{% endtab %}
{% endtabs %}
