---
description: >-
  Example code for the suix_getStakes JSON-RPC method. Complete guide on how to
  use suix_getStakes JSON-RPC in GetBlock Web3 documentation.
---

# suix\_getStakes - Sui

This method returns all delegated stakes for an address on the SUI network, including validator addresses, stake amounts, activation epochs, and estimated rewards.

### Parameters

| Parameter | Type   | Required | Description              | Default/Supported Values |
| --------- | ------ | -------- | ------------------------ | ------------------------ |
| owner     | string | Yes      | The staker's SUI address | Valid SUI address        |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
  "jsonrpc": "2.0",
  "method": "suix_getStakes",
  "params": ["0x9c76d5157eaa77c41a7bfda8db98a8e8080f7cb53b7313088ed085c73f866f21"],
  "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code overflow="wrap" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
  "jsonrpc": "2.0",
  "method": "suix_getStakes",
  "params": ["0x9c76d5157eaa77c41a7bfda8db98a8e8080f7cb53b7313088ed085c73f866f21"],
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
    .then(response => console.log(JSON.stringify(response.data)))
    .catch(error => console.log(error));
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code overflow="wrap" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
  "jsonrpc": "2.0",
  "method": "suix_getStakes",
  "params": ["0x9c76d5157eaa77c41a7bfda8db98a8e8080f7cb53b7313088ed085c73f866f21"],
  "id": "getblock.io"
})

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.text)
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code overflow="wrap" %}
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
              "method": "suix_getStakes",
              "params": ["0x9c76d5157eaa77c41a7bfda8db98a8e8080f7cb53b7313088ed085c73f866f21"],
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

### **Response**

{% code title="Response (truncated example)" overflow="wrap" %}
```json
{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": [
    {
      "validatorAddress": "0x3befb84f03a24386492bd3b05b1fd386172eb450e5059ce7df0ea6d9d6cefcaa",
      "stakingPool": "0x9a95cf69368e31b4dbe8ee9bdb3c0587bbc79d8fc6edf4007e185a962fd906df",
      "stakes": [
        {
          "stakedSuiId": "0xb4eeb46b70f0bebcae832aeef9f7c5db76052ab656e5f81853d0cf701cdbc8eb",
          "stakeRequestEpoch": "62",
          "stakeActiveEpoch": "63",
          "principal": "200000000000",
          "status": "Active",
          "estimatedReward": "520000000"
        }
      ]
    }
  ]
}
```
{% endcode %}

### Response Parameters

| Parameter                           | Type   | Description                              |
| ----------------------------------- | ------ | ---------------------------------------- |
| result\[].validatorAddress          | string | Validator's address                      |
| result\[].stakingPool               | string | Staking pool object ID                   |
| result\[].stakes\[].stakedSuiId     | string | Staked SUI object ID                     |
| result\[].stakes\[].principal       | string | Staked amount in MIST                    |
| result\[].stakes\[].status          | string | Stake status (Pending, Active, Unstaked) |
| result\[].stakes\[].estimatedReward | string | Estimated reward in MIST                 |

### Use Cases

1.  Staking Dashboard

    Display user's complete staking portfolio with rewards.
2.  Reward Tracking

    Monitor accumulated staking rewards over time.
3.  Validator Analysis

    View stake distribution across validators.

### &#x20;Errors Handling

| Error           | Description           | Solution                    |
| --------------- | --------------------- | --------------------------- |
| Invalid Address | Malformed SUI address | Verify address format       |
| Empty Result    | No stakes found       | Address may not have staked |

### SDK Integration

{% code title="example.ts" %}
```typescript
import { SuiClient } from '@mysten/sui/client';
const client = new SuiClient({ url: 'https://go.getblock.io/<ACCESS-TOKEN>/' });
const stakes = await client.getStakes({
  owner: '0x9c76d5157eaa77c41a7bfda8db98a8e8080f7cb53b7313088ed085c73f866f21'
});
console.log(stakes);
```
{% endcode %}
