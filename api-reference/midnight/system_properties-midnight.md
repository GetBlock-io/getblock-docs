---
description: >-
  Example code for the system_properties JSON-RPC method. Complete guide on how
  to use system_properties JSON-RPC in GetBlock Web3 documentation.
---

# system\_properties - Midnight

This method returns chain-specific properties such as SS58 address format, token decimals, and token symbols. For Midnight this also includes implementation-specific fields like the genesis transaction.

## Parameters

* None

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "system_properties",
    "params": [],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "system_properties",
    "params": [],
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
    "method": "system_properties",
    "params": [],
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
        "method": "system_properties",
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
{% endtab %}
{% endtabs %}

## Response Example

{% code overflow="wrap" %}
```json
{
    "jsonrpc": "2.0",
    "result": {
        "genesis_extrinsics": [
            "b4050600a46d69646e696768743a73797374656d2d7472616e73616374696f6e5b76365d3a050f0080c6a47e8d03",  "750305050061036d69646e696768743a7472616e73616374696f6e5b76395d287369676e61747572655b76315d2c70726f6f662c706564657273656e2d7363686e6f72725b76315d293a04004502011c70726570726f640b00203d88792d35e3bad57cb169ad35a2d7e3072638ba36217a1738cbf8a2aa8ff19f1f8d7748a70316cff92b35b129a9844aa82ec19e5ff6d2643be7a7aba56ab8116ffe73a25f71112e655b765ecc18264b6618d056061ac37698c01683310b7d27442bf5e3cdbb2c8f52945428c2a37e046e24b90771da253b1cb3f5f56f8daab587d4788100",
            "750305050061036d69646e696768743a7472616e73616374696f6e5b76395d287369676e61747572655b76315d2c70726f6f662c706564657273656e2d7363686e6f72725b76315d293a04004502011c70726570726f640b00203d88792d63876a1a13bfc6a282385b7eb186ddc60009492ab1648ef3d816798e44d64aec1bda0b3c56e2f522d7dd922749b2e8ec35a443cf8e21844641efb5da81105981ea297c36e68854b258b9a1a851990b76659909f9a22aa0d16d975f56d9b25314a22d496691b998c0fc7ebf39b2cb7ed26268aa854ee8eaf7e99178703818f63f00",
            "6d0b050600590b6d69646e696768743a73797374656d2d7472616e73616374696f6e5b76365d3a00c64a0600008e0a8c0072618b007eed60004ec66a008ef07200feaf7200f6d97c00a247090156426700c647eb00161f30005a1469008a7ab1005e5f74005298b100762490008abe8d008e4d7300d208840000466dba00e507fed717b0b25a040000163c060000da4f5000d2726f009e0e650066552a0015065ec12a000d068abc7700d27877000a1c10007ec71a00ba0cab000afb1800567e9300f200100052dd1a005e58ab00aa0f19007a1094005aee26001c624d060000824c9500763c8c000a4b7600e96df69ca80015660a5a81019d64004e4f2f0249c07212bd00004a586c0392ab6e175ee1200251c10241bd0000f2613903421045170a87e60012d65f009248a200b6e55b00664599022a1a01005aa30100b622240200b56bd2f661026e4d760385fa00fe75470259f5ed2366280c03fe61940286150100314a1e86fa01aa46010049209e8b6402d6606e0389fc00e2e21f029d4c3d427e550703e6704c0a0e910200ce3a140100b28587038a891f17226ffc05d6f96506ea8201003a2a43010082f361028aa450187e622b066e3f060002ff3217d54926142a420320c6305b034df61ec3f2051601aa90d15012f316001e3e791e82759d140076447102ea503eb0bae1912312eea6549608753602127a0002fd431410000284d71700000200400002127a000700d6117e030b00204aa9d1010b00204aa9d10102093d00420d030002c2eb0b000000000000008000000000000000000700f2052a012d81302a000000000000000000000000000000000000000000000a0000000000000000000000000000000100000000000000000000000000000001000000000000000000000000000000010000000000000000000000000000000100000000000000007512000000000000000000000000000000000000000040000000000000000000000000000000006400000000000000d107a10f",
    },
    "id": "getblock.io"
}

```
{% endcode %}

## Response Parameters

| Field                | Type             | Description                                               |
| -------------------- | ---------------- | --------------------------------------------------------- |
| result.ss58Format    | integer          | SS58 address format prefix used by the chain              |
| result.tokenDecimals | array of integer | Decimal places of each native token                       |
| result.tokenSymbol   | array of string  | Symbols of the native tokens (e.g. `["DUST"]`)            |
| result.genesis\_tx   | string           | Midnight-specific genesis transaction blob (when present) |

## Use Cases

* Configuring address encoders in wallet and dApp UIs
* Formatting balances with the correct decimal precision
* Detecting Midnight-specific extensions during chain bootstrap

## Error Handling

| Status Code | Error Message     | Cause                                       |
| ----------- | ----------------- | ------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`         |
| -32602      | Invalid params    | Request parameters are missing or malformed |
| 429         | Too Many Requests | Rate limit exceeded for your plan           |

## SDK Integration

{% tabs %}
{% tab title="@polkadot/api (JavaScript)" %}
```javascript
import { ApiPromise, WsProvider } from '@polkadot/api';

const wsProvider = new WsProvider('wss://go.getblock.io/<ACCESS-TOKEN>/');
const api = await ApiPromise.create({ provider: wsProvider });

// Call via the typed API:
const result = await api.rpc.system.properties();
console.log(result.toJSON());

// Or call the raw JSON-RPC method:
const raw = await api.rpc.rpc.methods();
// const raw = await (api as any)._rpcCore.provider.send('system_properties', []);
```
{% endtab %}

{% tab title="substrate-interface (Python)" %}
```python
from substrateinterface import SubstrateInterface

substrate = SubstrateInterface(url='wss://go.getblock.io/<ACCESS-TOKEN>/')

# Generic RPC call:
result = substrate.rpc_request('system_properties', [])
print(result)
```
{% endtab %}
{% endtabs %}
