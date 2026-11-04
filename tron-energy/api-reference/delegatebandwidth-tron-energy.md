---
description: Example code for the delegate-bandwidth TRON Energy API endpoint
---

# delegate-bandwidth - TRON energy

This is used to purchase and delegate TRON Bandwidth to a target address. Unlike Energy, Bandwidth orders are confirmed asynchronously: the API returns `202` with status `"accepted"`, and no charge is applied until on-chain delivery is confirmed. Poll [orders/{order\_id}](orders-order_id-tron-energy.md) with the returned `orderId` until the status becomes `"charged"`.

### Body Parameter

| Parameter       | Type    | Required | Description                                        |
| --------------- | ------- | -------- | -------------------------------------------------- |
| target\_address | string  | Yes      | TRON wallet address (starts with T, 34 characters) |
| volume          | integer | Yes      | Bandwidth amount: 1,000 — 50,000                   |
| duration        | string  | Yes      | `"1h"`, `"3d"`, `"7d"`, `"14d"`                    |

### Idempotency

This endpoint charges your balance and places an on-chain order. To make retries safe, send a unique `Idempotency-Key` header with each _intent_:

```
Idempotency-Key: 6f9c2a1e-3b7d-4c08-9f21-2e5a7c0b1d44
```

A UUID is recommended. Generate a **new** key for each purchase, and reuse the same key when retrying the exact same purchase.

| Situation                                         | Result                                                                                                                       |
| ------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| Same key + **same** body                          | The original response is replayed with the same `orderId`. No second charge, no second order.                                |
| Same key + **different** body                     | `422` — `Idempotency-Key was previously used with a different request body. Generate a fresh key per distinct request.`      |
| Same key on a **different** endpoint              | `409` — `Idempotency-Key was already used on a different endpoint (delegate-energy). Generate a fresh key per distinct intent.` |
| Duplicate sent while the first is still in flight | `409` with `Retry-After: 2` — `A request with this Idempotency-Key is already in progress. Retry shortly.`                   |

{% hint style="info" %}
Always send an idempotency key on write endpoints. It is the difference between a safe retry and an accidental double on-chain charge.
{% endhint %}

### Request Sample

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl -X POST https://services.getblock.io/v1/tron-energy/delegate-bandwidth \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -H "Idempotency-Key: 6f9c2a1e-3b7d-4c08-9f21-2e5a7c0b1d44" \
  -d '{
  "target_address": "TUo8pycbvje9w2XYsNnnzw67bpPs4GLFyD",
  "volume": 1000,
  "duration": "1h"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code overflow="wrap" %}
```javascript
import axios from 'axios';

const response = await axios.post(
  'https://services.getblock.io/v1/tron-energy/delegate-bandwidth',
  {
    "target_address": "TUo8pycbvje9w2XYsNnnzw67bpPs4GLFyD",
    "volume": 1000,
    "duration": "1h"
  },
  {
    headers: {
      'Content-Type': 'application/json',
      Authorization: 'Bearer YOUR_API_KEY',
      'Idempotency-Key': '6f9c2a1e-3b7d-4c08-9f21-2e5a7c0b1d44',
    },
  }
);
console.log(response.data);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code overflow="wrap" %}
```python
import requests

url = "https://services.getblock.io/v1/tron-energy/delegate-bandwidth"
payload = {
  "target_address": "TUo8pycbvje9w2XYsNnnzw67bpPs4GLFyD",
  "volume": 1000,
  "duration": "1h"
}
headers = {
    'Content-Type': 'application/json',
    'Authorization': 'Bearer YOUR_API_KEY',
    'Idempotency-Key': '6f9c2a1e-3b7d-4c08-9f21-2e5a7c0b1d44',
}

response = requests.post(url, headers=headers, json=payload)
print(response.json())
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code overflow="wrap" %}
```rust
use reqwest::header;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = reqwest::Client::new();

    let response = client
        .post("https://services.getblock.io/v1/tron-energy/delegate-bandwidth")
        .header(header::AUTHORIZATION, "Bearer YOUR_API_KEY")
        .header("Idempotency-Key", "6f9c2a1e-3b7d-4c08-9f21-2e5a7c0b1d44")
        .json(&json!({
          "target_address": "TUo8pycbvje9w2XYsNnnzw67bpPs4GLFyD",
          "volume": 1000,
          "duration": "1h"
        }))
        .send()
        .await?;

    println!("{}", response.text().await?);
    Ok(())
}
```
{% endcode %}
{% endtab %}

{% tab title="GO" %}
{% code overflow="wrap" %}
```go
package main

import (
    "bytes"
    "encoding/json"
    "fmt"
    "io"
    "net/http"
)

func main() {
    payload, _ := json.Marshal(map[string]interface{}{
        "target_address": "TUo8pycbvje9w2XYsNnnzw67bpPs4GLFyD",
        "volume": 1000,
        "duration": "1h",
    })

    req, _ := http.NewRequest("POST", "https://services.getblock.io/v1/tron-energy/delegate-bandwidth", bytes.NewBuffer(payload))
    req.Header.Set("Authorization", "Bearer YOUR_API_KEY")
    req.Header.Set("Content-Type", "application/json")
    req.Header.Set("Idempotency-Key", "6f9c2a1e-3b7d-4c08-9f21-2e5a7c0b1d44")

    resp, err := http.DefaultClient.Do(req)
    if err != nil {
        panic(err)
    }
    defer resp.Body.Close()

    body, _ := io.ReadAll(resp.Body)
    fmt.Println(string(body))
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

### Response Sample

{% tabs %}
{% tab title="202 (Accepted)" %}
```json
{
  "data": {
    "orderId": "22133650-7b1f-4793-8609-3c5f5481805c",
    "status": "accepted",
    "target_address": "TUo8pycbvje9w2XYsNnnzw67bpPs4GLFyD",
    "resource_type": "bandwidth",
    "volume": 1000,
    "duration": "1h",
    "price_usd": "",
    "trx_spent": 0,
    "txid": ""
  }
}
```

`price_usd` and `txid` are empty until delivery is confirmed. A retry with the same `Idempotency-Key` replays the same `202` — it does not create a second order.
{% endtab %}

{% tab title="Order after confirmation" %}
`GET /orders/22133650-7b1f-4793-8609-3c5f5481805c` a few seconds later:

```json
{
  "data": {
    "id": "22133650-7b1f-4793-8609-3c5f5481805c",
    "resource_type": "bandwidth",
    "target_address": "TUo8pycbvje9w2XYsNnnzw67bpPs4GLFyD",
    "status": "charged",
    "volume": 1000,
    "duration": "1h",
    "txid": "a5518c135f404f75c0d83d9f3bcb5131642356002457f11125f6bba97a6bbd1d",
    "price_usd": "0.33",
    "created_at": "2026-11-03T10:52:14Z",
    "updated_at": "2026-11-03T10:52:21Z",
    "charged_at": "2026-11-03T10:52:21Z"
  }
}
```
{% endtab %}
{% endtabs %}
