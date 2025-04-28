---
description: >-
  Retrieve a list of available accounts using the eth_accounts method via the
  JSON-RPC API Interface in the BSC protocol.
---

# eth\_accounts - Binance Smart Chain

{% hint style="success" %}
The RPC method retrieves a list of addresses owned by the client on the Binance Smart Chain, primarily for account management and transaction purposes.
{% endhint %}

The `eth_accounts` method in the BSC protocol is a part of the `eth_accounts` Web3 interface, which retrieves a list of addresses controlled by the client. This method is particularly useful for decentralized applications that need to display or manage user accounts without exposing private keys.

In the context of the `eth_accounts` RPC protocol, this method is invoked to interact with the blockchain via remote procedure calls, providing a seamless integration for developers to access user account information. By returning an array of account addresses, it facilitates efficient account management within the BSC ecosystem.

### Supported Networks

The `eth_accounts` JSON-RPC API method supports the following network types:

* **Mainnet**
* **Testnet**

### Parameters

None: This method does not require any parameters.

## Request Example

**API Endpoint**

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

**Request**

Here’s a sample cURL request using `eth_accounts` :

{% tabs %}
{% tab title="curl" %}
```json
curl --location --request POST https://go.getblock.io/<ACCESS-TOKEN>/
--header 'Content-Type: application/json' 
--data-raw {
  "jsonrpc": "2.0",
  "method": "eth_accounts",
  "params": [],
  "id": "getblock.io"
}
```
{% endtab %}
{% endtabs %}

**Response**

Below is a sample JSON response returned by `eth_accounts` upon a successful call:

```json

{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": []
}

```

### Body Parameters

Here is the list of body parameters for the `eth_accounts` method:

1. **jsonrpc**: The version of the JSON-RPC protocol used. It is typically `"2.0"`.
2. **id**: An identifier for the request, used to match responses to requests. In this case, it is `"getblock.io"`.
3. **result**: An array that contains the list of accounts. In this example, it is an empty array `[]`, indicating no accounts are present.

### Use Cases

Here are some use-cases for `eth_accounts` method:

1. **Retrieving User Accounts**: The `eth_accounts` method is commonly used to retrieve a list of accounts managed by the client's Ethereum node. This is particularly useful in Web3 applications where you need to display or interact with the user's available Ethereum addresses. For instance, when a decentralized application (dApp) is initialized, it can call `eth_accounts` to list all the accounts that the user can use for transactions or contract interactions.
2. **Account Management**: In scenarios where an application needs to manage multiple accounts, such as a wallet application, `eth_accounts` can be used to get a complete list of all accounts available on the node. This allows the application to provide functionalities like account selection, balance checks, and transaction history for each account.
3. **User Authentication**: Some dApps use `eth_accounts` as part of their user authentication process. By retrieving the user's accounts, the application can verify the user's identity and ensure that they have control over the accounts they claim to own. This is often combined with signing messages using the private keys associated with these accounts to prove ownership.

### Code for eth\_accounts

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
  "id": "getblock.io",
  "result": []
}

response = requests.post(url, headers=headers, data=json.dumps(payload))

# Check the response and print the result
if response.status_code == 200:
    print("Result:", response.json().get("result"))
else:
    print("Error:", response.status_code, response.text)

```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

const url = "https://go.getblock.io/<ACCESS-TOKEN>/";
const payload = {
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": []
};

axios.post(url, payload, {
  headers: { "Content-Type": "application/json" }
})
.then(response => {
  console.log("Result:", response.data.result);
})
.catch(error => {
  if (error.response) {
    console.error("Error:", error.response.status, error.response.data);
  } else {
    console.error("Request failed:", error.message);
  }
});
```
{% endtab %}
{% endtabs %}

### Common Errors

When using the `eth_accounts` JSON-RPC API BSC method, the following issues may occur:

* **Empty Account List**: If the returned account list is empty, ensure that your wallet is properly connected and unlocked. Check the connection settings and permissions in your Web3 provider.
* **Network Mismatch**: If accounts are not displaying, verify that you are connected to the correct BSC network. Mismatched networks between your wallet and the application can prevent account retrieval.
* **Provider Configuration Error**: An improperly configured Web3 provider can lead to method call failures. Double-check the provider settings to ensure it is correctly pointing to a valid BSC node.
* **Outdated Web3 Library**: Using an outdated version of the Web3.js library might cause compatibility issues. Update to the latest version to ensure smooth operation and support for the `eth_accounts` method.

Utilizing the `eth_accounts` method in Web3 applications provides a seamless way to access user accounts, enabling developers to interact with blockchain identities efficiently. This method is essential for verifying user presence and permissions, facilitating a more integrated and responsive decentralized application experience.

### Conclusion

The `eth_accounts` JSON-RPC method is crucial for retrieving a list of accounts controlled by the client, which is essential for managing assets on Ethereum and compatible networks like BSC. By using `eth_accounts`, developers can efficiently access account information, streamlining operations in decentralized applications. This method plays a vital role in the seamless integration and functionality of blockchain solutions.
