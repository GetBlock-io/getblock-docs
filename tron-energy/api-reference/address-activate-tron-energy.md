---
description: >-
  Example code for the addressActivate JSON RPC method. Сomplete guide on how to
  use addressActivate JSON RPC in GetBlock Web3 documentation.
---

# address-activate - TRON energy

This activates (creates) a TRON address on-chain. This is required before delegating Energy or Bandwidth to a new (never-used) address.\
A newly generated TRON address must be activated before it\
can hold a balance or receive transfers. Activation is synchronous: the API returns 200 with the provider's\
activation receipt, and the realised on-chain cost is charged from your prepaid balance.&#x20;

{% hint style="info" %}
Fixed cost: 1.87 TRX, charged in USD at the current TRX/USD rate.
{% endhint %}

### Body Parameter

| Parameter       | Type   | Required | Description                                            |
| --------------- | ------ | -------- | ------------------------------------------------------ |
| target\_address | string | Yes      | TRON wallet to activate (starts with T, 34 characters) |

### Request Sample

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl -X POST https://services.getblock.io/v1/tron-energy/address-activate \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -H "Idempotency-Key: 6f9c2a1e-3b7d-4c08-9f21-2e5a7c0b1d44" \
  -d '{
    "target_address": "TUo8pycbvje9w2XYsNnnzw67bpPs4GLFyD"
  }'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code overflow="wrap" %}
```javascript
import axios from 'axios';
const data = JSON.stringify({
    "target_address": "TUo8pycbvje9w2XYsNnnzw67bpPs4GLFyD"
  }
);

const config = {
    method: 'post',
    url: 'https://services.getblock.io/v1/tron-energy/address-activate',
    headers: {
        'Content-Type': 'application/json',
        'Authorization: Bearer YOUR_API_KEY'
        'Idempotency-Key: 6f9c2a1e-3b7d-4c08-9f21-2e5a7c0b1d44' \
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

url = "https://services.getblock.io/v1/tron-energy/address-activate"

payload = json.dumps({
    "target_address": "TUo8pycbvje9w2XYsNnnzw67bpPs4GLFyD"
  }
)

headers = {
        'Content-Type': 'application/json',
        'Authorization: Bearer YOUR_API_KEY',
        'Idempotency-Key: 6f9c2a1e-3b7d-4c08-9f21-2e5a7c0b1d44' 
    },
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

    let url = "https://services.getblock.io/v1/tron-energy/address-activate";
    let payload = r#"{
    "target_address": "TUo8pycbvje9w2XYsNnnzw67bpPs4GLFyD"
  }
"#;

    let response = client
        .post(url)
        .header(header::CONTENT_TYPE, "application/json")
        .header(header::AUTHORIZATION, "Bearer YOUR_API_KEY")
        .header(header::IDEMPOTENCY_KEY: "6f9c2a1e-3b7d-4c08-9f21-2e5a7c0b1d44")
        .body(payload)
        .send()
        .await?;

    println!("{}", response.text().await?);

    Ok(())
}
```
{% endcode %}
{% endtab %}

{% tab title="GO" %}
```go
package main
import (
    "bytes"
    "encoding/json"
    "fmt"
    "net/http"
)
func main() {
    url := "https://services.getblock.io/v1/tron-energy/address-activate"
    payload := map[string]interface{}{
    "target_address": "TUo8pycbvje9w2XYsNnnzw67bpPs4GLFyD"
  }

    jsonData, _ := json.Marshal(payload)
    req, _ := http.NewRequest("POST", url, bytes.NewBuffer(jsonData))
    req.Header.Set("Authorization", "Bearer YOUR_API_KEY")
    req.Header.Set("Content-Type", "application/json")
    req.Header.Set("Idempotency-Key": "6f9c2a1e-3b7d-4c08-9f21-2e5a7c0b1d44")
    client := &http.Client{}
    resp, err := client.Do(req)
    if err != nil {
        panic(err)
    }
    defer resp.Body.Close()
    
    var result map[string]interface{}
    json.NewDecoder(resp.Body).Decode(&result)
    fmt.Println(result)
}
```
{% endtab %}
{% endtabs %}

### Response Sample

```bash
{
"data": {
"id": "a1b2c3d4-5e6f-7a8b-9c0d-1e2f3a4b5c6d",
"address_to": "TUo8pycbvje...zw67bpPs4GLFyD",
"cost": 1700000,
"created_at": "2026-06-18T10:42:00Z"
        }
}
```

### Response Sample Definition

| Field       | Type    | Charged | Description                                                                 |
| ----------- | ------- | ------- | --------------------------------------------------------------------------- |
| id          | string  | No      | provider activation/order id                                                |
| address\_to | string  | No      | The TRON adrress that was activated                                         |
| cost        | integer | Yes     | realised on-chain cost in sun(1 TRX = 1,000,000 sun); this what is deducted |
| created\_at | string  | No      | RFC3339 timestamp of the activation                                         |

### Idempotency

Send a unique Idempotency-Key header (UUID format recommended) to ensure safe retries:

| Scenario                        | Behavior                                         |
| ------------------------------- | ------------------------------------------------ |
| Same key + same body            |  Original response replayed, no duplicate charge |
| Same key + different body       | 422                                              |
| Same key on different endpoint  | 409                                              |
| In-flight duplicate request     | 409 with Retry-After header                      |

A retry with the same key + body replays the original activation receipt — it does not activate the address twice or charge a second time.
