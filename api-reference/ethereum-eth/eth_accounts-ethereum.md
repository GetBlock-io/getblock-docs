---
description: >-
  Retrieve a list of Ethereum account addresses owned by the client using
  eth_accounts. Useful for displaying available accounts in dApps without
  requesting wallet access.
---

# eth\_accounts - Ethereum

{% hint style="success" %}
The **eth\_accounts** method in the **Ethereum JSON-RPC** protocol retrieves a list of Ethereum account addresses owned by the client.
{% endhint %}

This method is particularly useful in **Web3 applications** that need to access and display the available accounts for actions such as signing transactions or interacting with smart contracts. Unlike **eth\_requestAccounts**, which is used to request access to accounts in a MetaMask-enabled Web3 environment, **eth\_accounts** simply returns accounts already available to the client.

### Supported Networks

The eth\_accounts **RPC Ethereum** method supports the following network types

* **Mainnet**
* **Testnet**: Sepolia, Hoodi

### Parameters

{% hint style="info" %}
This method does not accept any parameters
{% endhint %}

### Request

#### URL

{% code fullWidth="false" %}
```json
https://go.getblock.io/<ACCESS-TOKEN>/
```
{% endcode %}

To retrieve the list of accounts, use the following eth\_accounts request with the JSON-RPC API

{% tabs %}
{% tab title="curl " %}
```json
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' 
--header 'Content-Type: application/json' 
--data-raw {
    "jsonrpc": "2.0",
    "method": "eth_accounts",
    "params": [],
    "id": "getblock.io"
}
```
{% endtab %}

{% tab title="wss" %}
```json
wscat -c wss://go.getblock.io/<ACCESS-TOKEN>/
# wait for connection and send the request body 
{"jsonrpc": "2.0",
"method": "eth_accounts",
"params": [],
"id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

### Response

Here’s a sample response for the eth\_accounts method, providing an array of Ethereum addresses

```json
{
    "result": "null",
    "id": "getblock.io",
    "status_code": 405,
    "message": "Method not allowed"
}
```

### Body Params

{% hint style="info" %}
Since eth\_accounts does not require body parameters, it is often a straightforward call to integrate when accessing the client’s accounts for Web3 applications.
{% endhint %}

### Use Case

The **eth\_accounts** method is commonly used in **dApps** and **Web3 applications** to retrieve a list of Ethereum accounts already accessible to the client. It allows applications to display accounts for actions like sending **transactions**, signing data, or interacting with **Ethereum smart contracts** without requiring additional user permissions each time.

In contrast, **eth\_requestAccounts** requests explicit user permission to access accounts, typically used when the app first needs access or when permissions are revoked. The main difference is that **eth\_accounts** returns already available accounts, while **eth\_requestAccounts** prompts the user for approval.

Understanding this distinction is important for creating secure Web3 applications, as **eth\_accounts** offers a seamless user experience, whereas **eth\_requestAccounts** ensures user control over wallet access.\\

### Code Example

Here is an example of how to use the eth\_accounts method in Python to retrieve a list of client-owned addresses

{% tabs %}
{% tab title="Python" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"
headers = {
    "Content-Type": "application/json"
}
payload = {
    "jsonrpc": "2.0",
    "method": "eth_accounts",
    "params": [],
    "id": "getblock.io"
}

response = requests.post(url, headers=headers, data=json.dumps(payload))

# Check the response and print the list of accounts
if response.status_code == 200:
    accounts = response.json().get("result", [])
    print("Available Accounts:", accounts)
else:
    print("Error:", response.status_code, response.text)

```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

// Define the API URL and headers
const url = "https://go.getblock.io/<ACCESS-TOKEN>/";
const headers = { "Content-Type": "application/json" };

// Prepare the request payload
const payload = {
    jsonrpc: "2.0",
    method: "eth_accounts",
    params: [],
    id: "getblock.io"
};

// Send the POST request
axios.post(url, payload, { headers })
    .then(response => {
        if (response.status === 200) {
            // Extract the list of accounts from the response
            const accounts = response.data.result || [];
            console.log("Available Accounts:", accounts);
        } else {
            // Handle unexpected HTTP statuses
            console.error("Error:", response.status, response.statusText);
        }
    })
    .catch(error => {
        // Handle network or server errors
        console.error("Error:", error.response ? error.response.data : error.message);
    });

```
{% endtab %}
{% endtabs %}

In this Python example, we use **eth\_accounts** to retrieve the list of accounts currently accessible to the client. This functionality can be essential in **dApps** or **Web3** projects that need to interact with user wallets seamlessly without repeatedly requesting permissions from MetaMask or similar wallet extensions.In case of an error, such as when the **eth\_accounts** request fails, the response may contain an error code or message, which can help troubleshoot the issue. For example, a **405 Method Not Allowed** error indicates that the requested method is not allowed on the server. Handling **eth\_accounts errors** properly ensures the smooth functioning of **Web3** applications that depend on client accounts for interaction.
