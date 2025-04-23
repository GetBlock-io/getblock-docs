---
description: >-
  The eth_accounts method retrieves a list of addresses controlled by the
  connected client. Learn how eth_accounts API Interface works and how to
  integrate it in your Web3 applications.
---

# eth\_chainld - TRON

## **Description:**

\
The `eth_accounts` Web3 method is used to fetch Ethereum addresses from the client. Using `eth_accounts` RPC protocol, developers can manage user accounts securely in dApps and Web3 services.

### Parameters

* This method does **not require any parameters**

### Request and Response

Here are samples of requests with `eth_accounts` method:

**Request in `curl`:**

```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/jsonrpc' 
--header 'Content-Type: application/json' 
 '{"jsonrpc":"2.0","method":"eth_chainId","params":[],"id":79}', "id": "getblock.io"}
```

Here are samples of responses with `eth_accounts` method:

**Response:**

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": [
    "0x742d35Cc6634C0532925a3b844Bc454e4438f44e",
    "0x3f5CE5FBFe3E9af3971dD833D26BA9b5C936f0bE"
  ]
}
```

### Body Params

Here is the list of body parameters for `eth_accounts` method:

1. **jsonrpc** (required) – Must be `"2.0"`.
2. **method** (required) – Must be `"eth_accounts"`.
3. **params** (required) – An empty array `[]`.
4. **id** (required) – Unique identifier for the request.

### Use Case

Here are some use-cases for `eth_accounts` method:

* **Retrieve user's wallet addresses** in a dApp to display or prefill a form.
* **Authenticate users** based on their Ethereum account address.
* **Check wallet connection status** without needing to request signature or transaction approval.

### eth\_accounts Errors

Here are some samples of common `eth_accounts` method errors:

* **No addresses returned:** This can happen if the user is not connected or their wallet is locked.
* **Empty array response:** Often indicates that permission to access accounts hasn’t been granted yet (common with MetaMask).
* **Unsupported client:** Some providers may not implement `eth_accounts` correctly under the `eth_accounts RPC Ethereum` standard.

### Code Example

Here are code samples for `eth_accounts` method:

**Python Example:**

```python
import requests

url = "https://go.getblock.io/<ACCESS-TOKEN>/"
payload = {
    "jsonrpc": "2.0",
    "method": "eth_accounts",
    "params": [],
    "id": 1
}
headers = {"Content-Type": "application/json"}

response = requests.post(url, json=payload, headers=headers)
print(response.json())
```

**JavaScript Example:**

```javascript
const fetch = require('node-fetch');

const url = "https://go.getblock.io/<ACCESS-TOKEN>/jsonrpc";
const payload = {
    jsonrpc: "2.0",
    method: "eth_accounts",
    params: [],
    id: 1
};

fetch(url, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(payload)
})
.then(response => response.json())
.then(data => console.log(data));
```

***

In conclusion, the `eth_accounts` method plays a crucial role in Web3 development, enabling access to user accounts securely and efficiently. Understanding `Web3 eth_accounts`, `Ethereum eth_accounts`, and differences like `eth_accounts vs eth_requestaccounts` and `metamask eth_accounts` is essential for building seamless dApps.
