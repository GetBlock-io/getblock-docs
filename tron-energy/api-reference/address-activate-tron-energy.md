---
description: Example code for the address-activate TRON Energy API endpoint
---

# address-activate - TRON energy

This activates (creates) a TRON address on-chain. A newly generated TRON address must be activated before it can receive delegated Energy or Bandwidth. Activation is synchronous: the API returns `200` once the account exists on-chain, and the cost is charged from your prepaid balance.

{% hint style="info" %}
Activation costs about **2.5 TRX**, charged in USD at the current TRX/USD rate. The exact amount is returned in `price_trx` and `price_usd`.
{% endhint %}

### Body Parameter

| Parameter       | Type   | Required | Description                                            |
| --------------- | ------ | -------- | ------------------------------------------------------ |
| target\_address | string | Yes      | TRON wallet to activate (starts with T, 34 characters) |

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

const response = await axios.post(
  'https://services.getblock.io/v1/tron-energy/address-activate',
  {
    "target_address": "TUo8pycbvje9w2XYsNnnzw67bpPs4GLFyD"
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

url = "https://services.getblock.io/v1/tron-energy/address-activate"
payload = {
  "target_address": "TUo8pycbvje9w2XYsNnnzw67bpPs4GLFyD"
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
        .post("https://services.getblock.io/v1/tron-energy/address-activate")
        .header(header::AUTHORIZATION, "Bearer YOUR_API_KEY")
        .header("Idempotency-Key", "6f9c2a1e-3b7d-4c08-9f21-2e5a7c0b1d44")
        .json(&json!({
          "target_address": "TUo8pycbvje9w2XYsNnnzw67bpPs4GLFyD"
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
    })

    req, _ := http.NewRequest("POST", "https://services.getblock.io/v1/tron-energy/address-activate", bytes.NewBuffer(payload))
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

```json
{
  "data": {
    "status": "success",
    "target_address": "TUo8pycbvje9w2XYsNnnzw67bpPs4GLFyD",
    "price_trx": "2.5",
    "price_usd": "0.81"
  }
}
```

### Response Sample Definition

| Field           | Type   | Description                                  |
| --------------- | ------ | -------------------------------------------- |
| status          | string | `"success"` when the account exists on-chain |
| target\_address | string | The TRON address that was activated          |
| price\_trx      | string | Activation cost in TRX                       |
| price\_usd      | string | Amount charged to your balance               |

A retry with the same key + body replays the original response — it does not activate the address twice or charge a second time.
