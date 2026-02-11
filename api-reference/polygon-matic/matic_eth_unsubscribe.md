---
description: >-
  Example code for the eth_unsubscribe json-rpc method. Сomplete guide on how to
  use eth_unsubscribe json-rpc in GetBlock.io Web3 documentation.
---

# eth\_unsubscribe - Polygon

The **eth\_unsubscribe** method cancels a subscription created with eth\_subscribe. The subscription will no longer send notifications after this call.

{% hint style="info" %}
This method requires a WebSocket connection.
{% endhint %}

## Parameters

| Parameter      | Type   | Required | Description                                |
| -------------- | ------ | -------- | ------------------------------------------ |
| subscriptionId | string | Yes      | Subscription ID returned by eth\_subscribe |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_unsubscribe",
    "params": ["0x9cef478923ff08bf67fde6c64013158d"],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code title="example.js" %}
```javascript
import axios from 'axios';

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
    jsonrpc: '2.0',
    method: 'eth_unsubscribe',
    params: ["0x9cef478923ff08bf67fde6c64013158d"],
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

{% tab title="Python (requests)" %}
{% code title="example.py" %}
```python
import requests

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "method": "eth_unsubscribe",
    "params": ["0x9cef478923ff08bf67fde6c64013158d"],
    "id": "getblock.io"
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
print(response.json())
```
{% endcode %}
{% endtab %}

{% tab title="Rust (reqwest)" %}
{% code title="example.rs" %}
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
            "method": "eth_unsubscribe",
            "params": ["0x9cef478923ff08bf67fde6c64013158d"],
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

{% hint style="info" %}
This method requires a WebSocket connection for subscriptions created with eth\_subscribe. Use eth\_unsubscribe to cancel those subscriptions.
{% endhint %}

## Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": true
}
```

## Response Parameters

| Field   | Type   | Description                |
| ------- | ------ | -------------------------- |
| jsonrpc | string | JSON-RPC version (2.0)     |
| id      | string | Request identifier         |
| result  | varies | Boolean indicating success |

## Use Case

The eth\_unsubscribe method is useful for:

* **Subscription cleanup**
* **Resource management**

## Error Handling

| Status Code | Error Message   | Cause                           |
| ----------- | --------------- | ------------------------------- |
| 403         | Forbidden       | Missing or invalid ACCESS-TOKEN |
| -32600      | Invalid Request | Malformed request body          |
| -32602      | Invalid params  | Invalid method parameters       |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const result = await provider.send('eth_unsubscribe', ["0x9cef478923ff08bf67fde6c64013158d"]);
console.log('Result:', result);
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

const result = await client.request({
    method: 'eth_unsubscribe',
    params: ["0x9cef478923ff08bf67fde6c64013158d"]
});
console.log('Result:', result);
```
{% endcode %}
{% endtab %}
{% endtabs %}
