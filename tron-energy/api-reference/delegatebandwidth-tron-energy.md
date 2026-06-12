---
description: >-
  Example code for the delegate-bandwidth JSON RPC method. Сomplete guide on how
  to use delegate-bandwidth JSON RPC in GetBlock Web3 documentation.
---

# delegate-bandwidth - TRON energy

This is used to purchase and delegate TRON bandwidth to a target address. Unlike energy, bandwidth orders may be confirmed asynchronously: if the provider accepts the order but on-chain delivery is not yet confirmed, the API returns `202` Accepted and no charge is applied until delivery is confirmed.

### Body Parameter

| Parameter       | Type    | Required | Description                                        |
| --------------- | ------- | -------- | -------------------------------------------------- |
| target\_address | string  | Yes      | TRON wallet address (starts with T, 34 characters) |
| volume          | integer | Yes      | Bandwidth amount: 1,000 — 200,000                  |
| duration        | string  | Yes      | "1h" (only 1 hour supported for Bandwidth)         |

### Request Sample

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl -X POST https://services.getblock.io/v1/tron-energy/delegate-bandwidth \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "target_address": "TUo8pycbvje9w2XYsNnnzw67bpPs4GLFyD",
    "volume": 5000,
    "duration": "1h"
  }
'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code overflow="wrap" %}
```javascript
import axios from 'axios';
const data = JSON.stringify({
    "target_address": "TUo8pycbvje9w2XYsNnnzw67bpPs4GLFyD",
    "volume": 5000,
    "duration": "1h"
  }
);

const config = {
    method: 'post',
    url: 'https://services.getblock.io/v1/tron-energy/delegate-bandwidth',
    headers: {
        'Content-Type': 'application/json',
        'Authorization: Bearer YOUR_API_KEY'
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

url = "https://services.getblock.io/v1/tron-energy/delegate-bandwidth"

payload = json.dumps({
    "target_address": "TUo8pycbvje9w2XYsNnnzw67bpPs4GLFyD",
    "volume": 5000,
    "duration": "1h"
  }
)

headers = {
        'Content-Type': 'application/json',
        'Authorization: Bearer YOUR_API_KEY'
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

    let url = "https://services.getblock.io/v1/tron-energy/delegate-bandwidth";
    let payload = r#"{
    "target_address": "TUo8pycbvje9w2XYsNnnzw67bpPs4GLFyD",
    "volume": 5000,
    "duration": "1h"
  }
"#;

    let response = client
        .post(url)
        .header(header::CONTENT_TYPE, "application/json")
        .header(header::AUTHORIZATION, "Bearer YOUR_API_KEY")
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
    url := "https://services.getblock.io/v1/tron-energy/delegate-bandwidth"
    payload := map[string]interface{}{
    "target_address": "TUo8pycbvje9w2XYsNnnzw67bpPs4GLFyD",
    "volume": 5000,
    "duration": "1h"
  }

    jsonData, _ := json.Marshal(payload)
    req, _ := http.NewRequest("POST", url, bytes.NewBuffer(jsonData))
    req.Header.Set("Authorization", "Bearer YOUR_API_KEY")
    req.Header.Set("Content-Type", "application/json")
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

{% hint style="warning" %}
Bandwidth only supports "1h" duration.
{% endhint %}

### Response Sample

{% tabs %}
{% tab title="200(Success)" %}
```bash
{
  "order_id": "clxyz123...",
  "status": "success",
  "target_address": "TUo8pycbvje9w2XYsNnnzw67bpPs4GLFyD",
  "resource_type": "energy",
  "volume": 65000,
  "duration": "3d",
  "price_usd": "4.97",
  "trx_spent": 20.12,
  "provider_order_id": 12345,
  "txid": "abc123def...",
  "message": "Order completed successfully."
}
```
{% endtab %}

{% tab title="202(Accepted)" %}
{% code overflow="wrap" %}
```bash
{
  "data": {
  "status": "accepted",
  "target_address": "TUo8pycbvje...zw67bpPs4GLFyD",
  "resource_type": "bandwidth",
  "volume": 1000,
  "duration": "1h",
  "provider_order_id": "36898a97-a8b9-46ea-b4cb-38a81e28992d",
  "message": "Order accepted by provider, awaiting on-chain confirmation."
 }
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

### Idempotency

Send a unique Idempotency-Key header (UUID format recommended) to ensure safe retries:

| Scenario                        | Behavior                                         |
| ------------------------------- | ------------------------------------------------ |
| Same key + same body            |  Original response replayed, no duplicate charge |
| Same key + different body       | 422                                              |
| Same key on different endpoint  | 409                                              |
| In-flight duplicate request     | 409 with Retry-After header                      |

A retry of a 202 response replays the same 202 — it does not create a second order
