# eth\_getbalance tempo

{% hint style="warning" %}
**MODIFIED FROM STANDARD EVM.** Tempo has no native gas token — this method's behavior differs from standard Ethereum nodes. See the description below for the Tempo-specific semantics.
{% endhint %}

**Tempo-modified.** Always returns the large constant placeholder `0x9612084f0316e0ebd5182f398e5195a51b5ca47667d4c9b26c9b26c9b26c9b2` rather than an actual balance. **Tempo has no native gas token**, so a traditional native-balance query is meaningless. To query an account's actual holdings, call `balanceOf(address)` on the specific TIP-20 token contract (e.g. pathUSD, AlphaUSD, KlarnaUSD, USD1). The placeholder value is intentionally large so that wallets that fall back to displaying the native balance show something visibly unusual rather than silently displaying zero.

## Parameters

| Parameter      | Type   | Required | Description                                                                     |
| -------------- | ------ | -------- | ------------------------------------------------------------------------------- |
| address        | string | Yes      | 20-byte address to check for balance (ignored — always returns the placeholder) |
| blockParameter | string | Yes      | Block number in hex, or block tag (ignored — always returns the placeholder)    |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getBalance",
    "params": [
        "0xd85498dbeaeb1df24be52eed4f52eac2fbd56245",
        "latest"
    ],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "eth_getBalance",
    "params": [
        "0xd85498dbeaeb1df24be52eed4f52eac2fbd56245",
        "latest"
    ],
    "id": "getblock.io"
});

const config = {
    method: 'post',
    url: 'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers: {
        'Content-Type': 'application/json'
    },
    data: data
};

axios(config)
    .then(response => console.log(JSON.stringify(response.data, null, 2)))
    .catch(error => console.log(error));
```
{% endtab %}

{% tab title="Python (Requests)" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "eth_getBalance",
    "params": [
        "0xd85498dbeaeb1df24be52eed4f52eac2fbd56245",
        "latest"
    ],
    "id": "getblock.io"
})

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.text)
```
{% endtab %}

{% tab title="Rust (Reqwest)" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();

    let payload = json!({
        "jsonrpc": "2.0",
        "method": "eth_getBalance",
        "params": [
                "0xd85498dbeaeb1df24be52eed4f52eac2fbd56245",
                "latest"
        ],
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
{% endtab %}
{% endtabs %}

## Response Example

{% code overflow="wrap" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x9612084f0316e0ebd5182f398e5195a51b5ca47667d4c9b26c9b26c9b26c9b2"
}
```
{% endcode %}

## Response Parameters

| Field  | Type   | Description                                                                                                    |
| ------ | ------ | -------------------------------------------------------------------------------------------------------------- |
| result | string | Always the constant placeholder shown above — not a real balance. Query TIP-20 `balanceOf` for actual holdings |

## Use Cases

* Compatibility shim for tooling that assumes the standard `eth_getBalance` exists
* Detecting that you're talking to a Tempo node (the placeholder is distinctive)

## Error Handling

| Status Code                 | Error Message     | Cause                                                                                   |
| --------------------------- | ----------------- | --------------------------------------------------------------------------------------- |
| 403                         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`                                                     |
| -32602                      | Invalid params    | Request parameters are missing or malformed                                             |
| -32601                      | Method not found  | Method does not exist or is not enabled on this node                                    |
| 429                         | Too Many Requests | Rate limit exceeded for your plan                                                       |
| (no method-specific errors) | N/A               | This method is a constant lookup and effectively cannot fail when the node is reachable |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const result = await provider.send('eth_getBalance', ["0xd85498dbeaeb1df24be52eed4f52eac2fbd56245", "latest"]);
console.log(result);
```
{% endtab %}

{% tab title="Viem" %}
```javascript
import { createPublicClient, http, defineChain } from 'viem';

// Tempo is not yet in viem/chains as a built-in — define it inline.
const tempo = defineChain({
    id: 4217,
    name: 'Tempo',
    network: 'tempo',
    // Tempo has no native gas token. The `nativeCurrency` field is required by viem's
    // type system; values shown here are a no-op placeholder — actual fees are paid in TIP-20.
    nativeCurrency: { name: 'USD Placeholder', symbol: 'USD', decimals: 18 },
    rpcUrls: {
        default: { http: ['https://go.getblock.io/<ACCESS-TOKEN>/'] }
    },
    blockExplorers: { default: { name: 'Tempo Explorer', url: 'https://explore.tempo.xyz' } }
});

const client = createPublicClient({
    chain: tempo,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const result = await client.request({ method: 'eth_getBalance', params: ["0xd85498dbeaeb1df24be52eed4f52eac2fbd56245", "latest"] });
console.log(result);
```
{% endtab %}
{% endtabs %}
