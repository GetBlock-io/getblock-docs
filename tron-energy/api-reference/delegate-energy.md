---
description: Example code for the delegate-energy JSON RPC method
---

# delegate-energy - TRON energy

This endpoint delegates energy to a TRON address. In most cases, the order completes immediately with status "success".

#### Parameter

| Parameter       | Type    | Required | Description                                        |
| --------------- | ------- | -------- | -------------------------------------------------- |
| target\_address | string  | Yes      | TRON wallet address (starts with T, 34 characters) |
| volume          | integer | Yes      | Energy amount: 30,000 — 5,000,000                  |
| duration        | string  | Yes      | "1h", "1d", "3d", "7d"                             |

### Idempotency

delegateEnergy charges your balance and may place an on-chain order. To make retries safe, send a unique key with each _intent_:

```
Idempotency-Key: 6f9c2a1e-3b7d-4c08-9f21-2e5a7c0b1d44
```

A UUID is recommended. Generate a **new** key for each purchase, and reuse the same key when retrying the exact same purchase.

| Situation                                         | Result                                                                |
| ------------------------------------------------- | --------------------------------------------------------------------- |
| Same key + **same** body                          | The original response is replayed. No second charge, no second order. |
| Same key + **different** body                     | `422`                                                                 |
| Same key on a **different** endpoint              | `409`                                                                 |
| Duplicate sent while the first is still in flight | `409` with a `Retry-After` header                                     |
| Idempotency store temporarily unavailable         | `503` with a `Retry-After` header (retry shortly)                     |

{% hint style="info" %}
Always send an idempotency key on write endpoints. It is the difference between a safe retry and an accidental double on-chain charge.
{% endhint %}

#### Request Sample

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl -X POST https://services.getblock.io/v1/tron-energy/delegate-energy \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -H "Idempotency-Key: 6f9c2a1e-3b7d-4c08-9f21-2e5a7c0b1d44" \
  -d '{
    "target_address": "TUo8pycbvje...zw67bpPs4GLFyD",
    "volume": 65000,
    "duration": "1h"
  }'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code overflow="wrap" %}
```javascript
import axios from 'axios';
const data = JSON.stringify({
    "target_address": "TUo8pycbvje...zw67bpPs4GLFyD",
    "volume": 65000,
    "duration": "1h"
  }
);

const config = {
    method: 'post',
    url: 'https://services.getblock.io/v1/tron-energy/delegate-energy',
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

url = "https://services.getblock.io/v1/tron-energy/delegate-energy"

payload = json.dumps({
    "target_address": "TUo8pycbvje...zw67bpPs4GLFyD",
    "volume": 65000,
    "duration": "1h"
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

    let url = "https://services.getblock.io/v1/tron-energy/delegate-energy";
    let payload = r#"{{
    "target_address": "TUo8pycbvje...zw67bpPs4GLFyD",
    "volume": 65000,
    "duration": "1h"
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
    url := "https://services.getblock.io/v1/tron-energy/delegate-energy"
    payload := map[string]interface{}{
    "target_address": "TUo8pycbvje...zw67bpPs4GLFyD",
    "volume": 65000,
    "duration": "1h"
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

#### &#x20;Response&#x20;

```bash
{
  "data": {
    "status": "success",
    "target_address": "TUo8...",
    "resource_type": "energy",
    "volume": 65000,
    "duration": "1h",
    "price_usd": "0.59",
    "trx_spent": 3,
    "order_id": "1H76d06176b8",
    "txid": "db47c131876a504ceee32f023a4aa3aa23629362e...",
    "message": "Delegation completed successfully."
  }
}
```
