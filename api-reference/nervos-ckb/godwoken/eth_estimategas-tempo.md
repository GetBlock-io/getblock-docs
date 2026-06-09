# eth\_estimategas tempo

{% hint style="warning" %}
**MODIFIED FROM STANDARD EVM.** Tempo has no native gas token — this method's behavior differs from standard Ethereum nodes. See the description below for the Tempo-specific semantics.
{% endhint %}

**Tempo-modified.** Estimates the gas required to execute a transaction. Unlike standard Ethereum, the gas allowance is calculated from the **effective fee payer's selected TIP-20 fee token balance**, not from a native ETH balance. The `from` address in the call object determines whose TIP-20 balance is checked; if the transaction uses fee sponsorship, the sponsoring address's balance is checked instead.

## Parameters

| Parameter      | Type   | Required | Description                                                               |
| -------------- | ------ | -------- | ------------------------------------------------------------------------- |
| callObject     | object | Yes      | Transaction-shaped object with `to`, `data`, and optional `from`, `value` |
| blockParameter | string | No       | Optional block parameter (default: `"latest"`)                            |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_estimateGas",
    "params": [
        {
            "from": "0xd85498dbeaeb1df24be52eed4f52eac2fbd56245",
            "to": "0x4242424242424242424242424242424242424242",
            "data": "0xa9059cbb000000000000000000000000d85498dbeaeb1df24be52eed4f52eac2fbd5624500000000000000000000000000000000000000000000000000000000000003e8"
        }
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
    "method": "eth_estimateGas",
    "params": [
        {
            "from": "0xd85498dbeaeb1df24be52eed4f52eac2fbd56245",
            "to": "0x4242424242424242424242424242424242424242",
            "data": "0xa9059cbb000000000000000000000000d85498dbeaeb1df24be52eed4f52eac2fbd5624500000000000000000000000000000000000000000000000000000000000003e8"
        }
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
    "method": "eth_estimateGas",
    "params": [
        {
            "from": "0xd85498dbeaeb1df24be52eed4f52eac2fbd56245",
            "to": "0x4242424242424242424242424242424242424242",
            "data": "0xa9059cbb000000000000000000000000d85498dbeaeb1df24be52eed4f52eac2fbd5624500000000000000000000000000000000000000000000000000000000000003e8"
        }
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
        "method": "eth_estimateGas",
        "params": [
                {
                        "from": "0xd85498dbeaeb1df24be52eed4f52eac2fbd56245",
                        "to": "0x4242424242424242424242424242424242424242",
                        "data": "0xa9059cbb000000000000000000000000d85498dbeaeb1df24be52eed4f52eac2fbd5624500000000000000000000000000000000000000000000000000000000000003e8"
                }
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
    "result": "0x12345"
}
```
{% endcode %}

## Response Parameters

| Field  | Type   | Description                                                                               |
| ------ | ------ | ----------------------------------------------------------------------------------------- |
| result | string | Estimated gas units required (hex). Convert to TIP-20 fee cost using the current base fee |

## Use Cases

* Setting an appropriate gas limit before signing a payment transaction
* Showing users an estimated cost in TIP-20 fee tokens in wallet UIs
* Detecting transactions that would revert (estimate fails)

## Error Handling

| Status Code | Error Message                  | Cause                                                                 |
| ----------- | ------------------------------ | --------------------------------------------------------------------- |
| 403         | Forbidden                      | Missing or invalid `<ACCESS-TOKEN>`                                   |
| -32602      | Invalid params                 | Request parameters are missing or malformed                           |
| -32601      | Method not found               | Method does not exist or is not enabled on this node                  |
| 429         | Too Many Requests              | Rate limit exceeded for your plan                                     |
| -32000      | Execution reverted             | Transaction would revert; estimate cannot be returned                 |
| -32000      | Insufficient fee token balance | Fee payer's selected TIP-20 fee token balance is too low to cover gas |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const result = await provider.send('eth_estimateGas', [{"from": "0xd85498dbeaeb1df24be52eed4f52eac2fbd56245", "to": "0x4242424242424242424242424242424242424242", "data": "0xa9059cbb000000000000000000000000d85498dbeaeb1df24be52eed4f52eac2fbd5624500000000000000000000000000000000000000000000000000000000000003e8"}]);
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

const result = await client.request({ method: 'eth_estimateGas', params: [{"from": "0xd85498dbeaeb1df24be52eed4f52eac2fbd56245", "to": "0x4242424242424242424242424242424242424242", "data": "0xa9059cbb000000000000000000000000d85498dbeaeb1df24be52eed4f52eac2fbd5624500000000000000000000000000000000000000000000000000000000000003e8"}] });
console.log(result);
```
{% endtab %}
{% endtabs %}
