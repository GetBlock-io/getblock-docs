---
description: Example code for the delegate-energy TRON Energy API endpoint
---

# delegate-energy - TRON energy

This endpoint delegates Energy to a TRON address. If the delegation lands immediately, the API returns `200` with status `"success"` and an on-chain `txid`. Otherwise it returns `202` with status `"accepted"` — poll [orders/{order\_id}](orders-order_id-tron-energy.md) with the returned `orderId` until the status becomes `"charged"` (7 seconds in our tests).

{% hint style="info" %}
The target address must already be activated on-chain. For a brand-new address, call [address-activate](address-activate-tron-energy.md) first.
{% endhint %}

#### Parameter

| Parameter       | Type    | Required | Description                                             |
| --------------- | ------- | -------- | ------------------------------------------------------- |
| target\_address | string  | Yes      | TRON wallet address (starts with T, 34 characters)      |
| volume          | integer | Yes      | Energy amount: 32,000 — 5,000,000                       |
| duration        | string  | Yes      | `"15m"`, `"1h"`, `"1d"`, `"3d"`, `"7d"`, `"14d"`        |

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
  "target_address": "TUo8pycbvje9w2XYsNnnzw67bpPs4GLFyD",
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

const response = await axios.post(
  'https://services.getblock.io/v1/tron-energy/delegate-energy',
  {
    "target_address": "TUo8pycbvje9w2XYsNnnzw67bpPs4GLFyD",
    "volume": 65000,
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

url = "https://services.getblock.io/v1/tron-energy/delegate-energy"
payload = {
  "target_address": "TUo8pycbvje9w2XYsNnnzw67bpPs4GLFyD",
  "volume": 65000,
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
        .post("https://services.getblock.io/v1/tron-energy/delegate-energy")
        .header(header::AUTHORIZATION, "Bearer YOUR_API_KEY")
        .header("Idempotency-Key", "6f9c2a1e-3b7d-4c08-9f21-2e5a7c0b1d44")
        .json(&json!({
          "target_address": "TUo8pycbvje9w2XYsNnnzw67bpPs4GLFyD",
          "volume": 65000,
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
        "volume": 65000,
        "duration": "1h",
    })

    req, _ := http.NewRequest("POST", "https://services.getblock.io/v1/tron-energy/delegate-energy", bytes.NewBuffer(payload))
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

#### Response

{% tabs %}
{% tab title="200 (Success)" %}
```json
{
  "data": {
    "orderId": "0a6d59c4-9486-45b1-bf3f-74d075ad9b4c",
    "status": "success",
    "target_address": "TUo8pycbvje9w2XYsNnnzw67bpPs4GLFyD",
    "resource_type": "energy",
    "volume": 65000,
    "duration": "1h",
    "price_usd": "0.40",
    "trx_spent": 2,
    "txid": "63285f35942a0a8552fa7664b5fd8d4c2140640e9c766fe44fda7e01c97233df"
  }
}
```
{% endtab %}

{% tab title="202 (Accepted)" %}
```json
{
  "data": {
    "orderId": "822448b4-88de-4a27-865b-5a2a17973ecd",
    "status": "accepted",
    "target_address": "TUo8pycbvje9w2XYsNnnzw67bpPs4GLFyD",
    "resource_type": "energy",
    "volume": 32000,
    "duration": "1h",
    "price_usd": "",
    "trx_spent": 0,
    "txid": ""
  }
}
```

`price_usd`, `trx_spent`, and `txid` are filled in once the order reaches `charged`.
{% endtab %}
{% endtabs %}

### Response Sample Definition

| Field           | Type    | Description                                                         |
| --------------- | ------- | ------------------------------------------------------------------- |
| orderId         | string  | Order ID. Pass it to [orders/{order\_id}](orders-order_id-tron-energy.md) |
| status          | string  | `"success"` (delivered, `200`) or `"accepted"` (pending, `202`)     |
| target\_address | string  | Address that received the Energy                                    |
| resource\_type  | string  | `"energy"`                                                          |
| volume          | integer | Energy delegated                                                    |
| duration        | string  | Rental period                                                       |
| price\_usd      | string  | Amount charged to your balance. Empty while `accepted`              |
| trx\_spent      | number  | TRX spent on the delegation. `0` while `accepted`                   |
| txid            | string  | Delegation transaction hash on TRON. Empty while `accepted`         |

{% hint style="info" %}
The on-chain amount can be rounded down by 1 unit (for example, 65,000 ordered shows as `EnergyLimit: 64999`). Add a small buffer if a transaction needs an exact amount.
{% endhint %}
