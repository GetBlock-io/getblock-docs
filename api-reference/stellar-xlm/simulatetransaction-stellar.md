---
description: >-
  Example code for the simulateTransaction JSON-RPC method. Complete guide on
  how to use simulateTransaction JSON-RPC in GetBlock Web3 documentation.
---

# simulateTransaction - Stellar

This method simulates a smart contract invocation without actually submitting it to the network. It's useful for testing and debugging transactions safely.

### Parameters

| Parameter      | Type   | Description                               |
| -------------- | ------ | ----------------------------------------- |
| transaction    | string | Transaction envelope (base64-encoded XDR) |
| resourceConfig | object | (optional) Resource configuration         |

Resource Config Object:

| Field             | Type    | Description                    |
| ----------------- | ------- | ------------------------------ |
| instructionLeeway | integer | Additional instructions buffer |

Request

{% tabs %}
{% tab title="cURL" %}
{% code title="curl" overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "simulateTransaction",
    "params": {
        "transaction": "AAAAAgAAAAAg4dbAxsGAGICfBG3iT2cKGYQ6hK4sJWzZ6or1C5v6GAAAAGQAJsOiAAAAEQAAAAEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAEAAAAAAAAAGAAAAAAAAAABzAP+dP0PsNzYvFF1pv7a8RQXwH5eg3uZBbbWjE9PwAsAAAAJaW5jcmVtZW50AAAAAAAAAgAAABIAAAAAAAAAACDh1sDGwYAYgJ8EbeJPZwoZhDqEriwlbNnqivULm/oYAAAAAwAAAAMAAAAAAAAAAAAAAAA=",
        "resourceConfig": {
            "instructionLeeway": 3000000
        }
    },
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="simulateTransaction.js" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "simulateTransaction",
    "params": {
        "transaction": "AAAAAgAAAAAg4dbAxsGAGICfBG3iT2cKGYQ6hK4sJWzZ6or1C5v6GAAAAGQAJsOiAAAAEQAAAAEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAEAAAAAAAAAGAAAAAAAAAABzAP+dP0PsNzYvFF1pv7a8RQXwH5eg3uZBbbWjE9PwAsAAAAJaW5jcmVtZW50AAAAAAAAAgAAABIAAAAAAAAAACDh1sDGwYAYgJ8EbeJPZwoZhDqEriwlbNnqivULm/oYAAAAAwAAAAMAAAAAAAAAAAAAAAA=",
        "resourceConfig": {
            "instructionLeeway": 3000000
        }
    },
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

{% tab title="Python" %}
{% code title="simulate_transaction.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "simulateTransaction",
    "params": {
        "transaction": "AAAAAgAAAAAg4dbAxsGAGICfBG3iT2cKGYQ6hK4sJWzZ6or1C5v6GAAAAGQAJsOiAAAAEQAAAAEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAEAAAAAAAAAGAAAAAAAAAABzAP+dP0PsNzYvFF1pv7a8RQXwH5eg3uZBbbWjE9PwAsAAAAJaW5jcmVtZW50AAAAAAAAAgAAABIAAAAAAAAAACDh1sDGwYAYgJ8EbeJPZwoZhDqEriwlbNnqivULm/oYAAAAAwAAAAMAAAAAAAAAAAAAAAA=",
        "resourceConfig": {
            "instructionLeeway": 3000000
        }
    },
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
{% code title="main.rs" %}
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
            "method": "simulateTransaction",
            "params": {
                "transaction": "AAAAAgAAAAAg4dbAxsGAGICfBG3iT2cKGYQ6hK4sJWzZ6or1C5v6GAAAAGQAJsOiAAAAEQAAAAEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAEAAAAAAAAAGAAAAAAAAAABzAP+dP0PsNzYvFF1pv7a8RQXwH5eg3uZBbbWjE9PwAsAAAAJaW5jcmVtZW50AAAAAAAAAgAAABIAAAAAAAAAACDh1sDGwYAYgJ8EbeJPZwoZhDqEriwlbNnqivULm/oYAAAAAwAAAAMAAAAAAAAAAAAAAAA=",
                "resourceConfig": {
                    "instructionLeeway": 3000000
                }
            },
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

Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "latestLedger": 2553978,
        "minResourceFee": "79488",
        "results": [
            {
                "xdr": "AAAAAwAAAAM=",
                "auth": []
            }
        ],
        "transactionData": "AAAAAAAAAAIAAAAGAc...",
        "cost": {
            "cpuInsns": "1234567",
            "memBytes": "12345"
        }
    }
}
```

### Response Parameters

| Field           | Type    | Description                                       |
| --------------- | ------- | ------------------------------------------------- |
| latestLedger    | integer | Latest ledger at simulation time                  |
| minResourceFee  | string  | Recommended minimum resource fee                  |
| results         | array   | Simulation results for each operation             |
| transactionData | string  | Recommended Soroban transaction data (base64 XDR) |
| cost            | object  | Resource cost breakdown                           |
| cpuInsns        | string  | CPU instructions consumed                         |
| memBytes        | string  | Memory bytes consumed                             |
| error           | string  | (optional) Error message if simulation failed     |

### Use Case

The simulateTransaction method is essential for:

* Smart contract testing before submission
* Fee estimation for Soroban transactions
* Transaction debugging
* Dry-run operations
* Resource requirement calculation
* Authorization preparation

### Error Handling

| Status Code      | Error Message  | Cause                           |
| ---------------- | -------------- | ------------------------------- |
| 403              | Forbidden      | Missing or invalid ACCESS-TOKEN |
| -32602           | Invalid params | Invalid transaction format      |
| Simulation error | Various        | Contract execution failure      |
