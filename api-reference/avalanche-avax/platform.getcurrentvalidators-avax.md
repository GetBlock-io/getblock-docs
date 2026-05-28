---
description: >-
  Example code for the platform.getCurrentValidators JSON-RPC method. Complete
  guide on how to use platform.getCurrentValidators JSON-RPC in GetBlock Web3
  documentation.
---

# platform.getCurrentValidators - AVAX

Returns the list of current validators for a given subnet (defaults to the Primary Network if `subnetID` is omitted). Each validator entry includes stake amount, start/end time, reward address, and delegators.

## Parameters

| Parameter | Type            | Required | Description                          |
| --------- | --------------- | -------- | ------------------------------------ |
| subnetID  | string          | No       | Subnet ID (default: Primary Network) |
| nodeIDs   | array of string | No       | Specific node IDs to filter by       |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/P' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "platform.getCurrentValidators",
    "params": {},
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "platform.getCurrentValidators",
    "params": {},
    "id": "getblock.io"
});

const config = {
    method: 'post',
    url: 'https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/P',
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

url = "https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/P"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "platform.getCurrentValidators",
    "params": {},
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
        "method": "platform.getCurrentValidators",
        "params": {},
        "id": "getblock.io"
});

    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/P")
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
    "result": {
        "validators": [
            {
                "txID": "2kxwWpHvZPhMsJcSTmM7a3Da7sExB8pPyF7t4cr2NSwnYqNHni",
                "startTime": "1700000000",
                "endTime": "1731536000",
                "stakeAmount": "2000000000000",
                "nodeID": "NodeID-7Xhw2mDxuDS44j42TCB6U5579esbSt3Lg",
                "weight": "2000000000000",
                "rewardOwner": {
                    "locktime": "0",
                    "threshold": "1",
                    "addresses": [
                        "P-avax1fw57u4tp7xzx0k6ufn7tj9caua59mt9gqcvy7m"
                    ]
                },
                "potentialReward": "12000000000",
                "delegationFee": "2.0000",
                "uptime": "0.9876",
                "connected": true,
                "delegators": []
            }
        ]
    }
}
```
{% endcode %}

## Response Parameters

| Field                              | Type   | Description                                    |
| ---------------------------------- | ------ | ---------------------------------------------- |
| result.validators                  | array  | Array of validator objects                     |
| result.validators\[].nodeID        | string | NodeID of the validator                        |
| result.validators\[].stakeAmount   | string | Amount staked in nAVAX (decimal string)        |
| result.validators\[].startTime     | string | Validation start time (Unix timestamp, string) |
| result.validators\[].endTime       | string | Validation end time (Unix timestamp, string)   |
| result.validators\[].uptime        | string | Validator's uptime as a 0-1 ratio              |
| result.validators\[].delegationFee | string | Delegation fee percentage                      |
| result.validators\[].delegators    | array  | Delegators bonded to this validator            |

## Use Cases

* Building staking dashboards
* Validator monitoring and performance tracking
* Computing total staked AVAX for analytics
* Subnet validator-set introspection

## Error Handling

| Status Code | Error Message     | Cause                                       |
| ----------- | ----------------- | ------------------------------------------- |
| 404         | Not Found         | Missing or invalid `<ACCESS-TOKEN>`         |
| -32602      | Invalid params    | Request parameters are missing or malformed |
| 429         | Too Many Requests | Rate limit exceeded for your plan           |

## SDK Integration

{% tabs %}
{% tab title="AvalancheJS / Axios" %}
```javascript
// AvalancheJS provides typed clients for P-Chain and X-Chain.
// P-Chain operations are exposed through `pvm` in AvalancheJS.
// For raw JSON-RPC access, use a generic HTTP client as in the Request Example.
import axios from 'axios';

const response = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/P', {
    jsonrpc: '2.0',
    id: 'getblock.io',
    method: 'platform.getCurrentValidators',
    params: {}
}});
console.log(response.data);
```
{% endtab %}

{% tab title="Python" %}
```python
# AvalancheJS is JavaScript-only. For Python, use a generic JSON-RPC client.
import requests
import json

url = 'https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/P'
payload = {
    'jsonrpc': '2.0',
    'id': 'getblock.io',
    'method': 'platform.getCurrentValidators',
    'params': {}
}}
response = requests.post(url, json=payload)
print(response.json())
```
{% endtab %}
{% endtabs %}
