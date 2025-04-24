---
description: >-
  Discover eth_accounts in the JSON-RPC API Interface for seamless interaction with Binance Smart Chain accounts.
---

# eth_accounts

{% hint style="success" %}
The RPC method retrieves a list of addresses owned by the client, primarily used to identify available accounts for transactions on BSC.&#x20;
{% endhint %}

The eth_accounts Web3 method in the Binance Smart Chain (BSC) JSON-RPC API Interface provides a straightforward way to retrieve a list of accounts managed by the client's wallet. When invoked, this method returns an array of addresses, representing the accounts available for transactions and other blockchain interactions. As part of the eth_accounts RPC protocol, it ensures efficient management of account data without exposing private keys. This method is particularly useful for developers building decentralized applications (dApps) on BSC, enabling them to access user accounts seamlessly. By integrating eth_accounts Web3, developers can enhance their applications' functionality, ensuring they align with blockchain standards and provide a robust user experience.

### Supported Networks

The eth_accounts REST API method supports the following network types
- **Mainnet**
- **Testnets**

### Parameters

None: This method does not require any parameters.

### Request Example

#### API Endpoint

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```
Here’s a sample cURL request using eth_accounts

#### Request

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

### Response


```json

{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": []
}

```

### Body Parameters

Here is the list of body parameters for eth_accounts method:

1. `jsonrpc`: The version of the JSON-RPC protocol used, typically "2.0".
2. `id`: An identifier for the request, which can be a string, number, or null. It is used to match responses to requests.
3. `result`: An array that contains the list of addresses controlled by the client. In this example, it is an empty array, indicating no accounts are available.

### Use Cases

Here are some use-cases for eth_accounts method in Web3 programming:

1. **Retrieving Ethereum Addresses**: This method is commonly used to retrieve a list of Ethereum addresses that are managed by a connected node. Developers can utilize this to display available accounts in a decentralized application (dApp) or to allow users to select which account they wish to use for transactions.

2. **Account Management**: In scenarios where a dApp needs to manage multiple accounts, this method can be employed to programmatically access and display all the accounts associated with a user's wallet. This is particularly useful for applications that require users to switch between different accounts for various operations.

3. **Testing and Development**: During the development and testing phase of dApps, developers often need to simulate transactions or interactions with smart contracts using different accounts. This method provides an easy way to access test accounts, especially when using development environments like Ganache, which automatically generates multiple accounts for testing purposes.

### Code for eth_accounts

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
{% endtabs %}

## Common Errors

Common Errors  
When using the eth_accounts JSON-RPC API BSC method, the following issues may occur:  
- Empty account list: This can happen if the client is not connected to a wallet or if the wallet is locked. Ensure that your wallet is properly connected and unlocked before making the request.  
- Network mismatch: If the client is connected to a different network than BSC, the accounts may not be retrieved. Verify that your client is configured to connect to the correct BSC network.  
- RPC endpoint issues: An incorrect or unreachable RPC endpoint can prevent account retrieval. Double-check the RPC URL and ensure network connectivity to the BSC node.  
- Unsupported client: Some clients or libraries may not fully support the eth_accounts method. Make sure to use a compatible client version or library that supports this method.

Using the eth_accounts method is beneficial in Web3 applications as it allows developers to programmatically retrieve a list of accounts managed by the connected client. This functionality is essential for applications that need to interact with user wallets, enabling seamless integration and management of blockchain accounts.

### conclusion

The eth_accounts method in the JSON-RPC API is an essential tool for retrieving a list of available accounts managed by the client. This function is crucial for developers working with Ethereum or Binance Smart Chain (BSC) as it allows them to access and manage account information efficiently. By utilizing eth_accounts, users can seamlessly integrate account management capabilities into their blockchain applications.
