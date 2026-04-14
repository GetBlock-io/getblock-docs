---
description: >-
  Example code for the addressActivate JSON RPC method. Сomplete guide on how to
  use addressActivate JSON RPC in GetBlock Web3 documentation.
---

# addressActivate - TRON energy

This activates a TRON address on the blockchain. Required before delegating Energy or Bandwidth to a new (never-used) address.

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
curl -X POST https://api.getblock.io/tron-energy/addressActivate \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
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
    url: 'https://api.getblock.io/tron-energy/addressActivate',
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

url = "https://api.getblock.io/tron-energy/addressActivate"

payload = json.dumps({
    "target_address": "TUo8pycbvje9w2XYsNnnzw67bpPs4GLFyD"
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

    let url = "https://api.getblock.io/tron-energy/addressActivate";
    let payload = r#"{
    "target_address": "TUo8pycbvje9w2XYsNnnzw67bpPs4GLFyD"
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
    url := "https://api.getblock.io/tron-energy/addressActivate"
    payload := map[string]interface{}{
    "target_address": "TUo8pycbvje9w2XYsNnnzw67bpPs4GLFyD"
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

### Response Sample

```bash
{
  "order_id": 12345,
  "status": "success",
  "target_address": "TUo8pycbvje9w2XYsNnnzw67bpPs4GLFyD",
  "trx_spent": 1.87,
  "price_usd": "0.52",
  "txid": "abc123def..."
}
```
