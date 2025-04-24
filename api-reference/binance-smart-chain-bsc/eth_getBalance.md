---
description: >-
  Retrieve account balances using eth_getBalance in the JSON-RPC API Interface on the BSC protocol for seamless blockchain interactions.
---

# eth_getBalance

{% hint style="success" %}
The RPC method retrieves the balance of a specified Binance Smart Chain address at a particular block height, primarily for checking account balances.&#x20;
{% endhint %}

The eth_getBalance Web3 method allows users to query the balance of an account at a specific block height using the BSC protocol. By leveraging the eth_getBalance RPC protocol, developers can obtain the current balance of any address in wei, the smallest denomination of BNB. This method requires two parameters: the account address and the block number or one of the predefined block tags such as "latest", "earliest", or "pending". The response returns a hexadecimal value representing the balance. This method is essential for applications needing real-time or historical balance checks, enabling efficient fund tracking and management in decentralized applications. The JSON-RPC API Interface ensures seamless integration with the Binance Smart Chain, facilitating robust blockchain interactions.

### Supported Networks

The eth_getBalance REST API method supports the following network types
- **Mainnet**
- **Testnets**

### Parameters

Here is the list of parameters eth_getBalance method needs to be executed. Do not highlight the method name or enclose it in quotation marks.

- **Parameter 1: Address**
  - **Type**: String
  - **Description**: The address of the account whose balance is being queried.
  - **Required**: Yes
  - **Default/Supported Values**: Must be a valid Ethereum address, typically a 40-character hexadecimal string prefixed with '0x'.

- **Parameter 2: Block Parameter**
  - **Type**: String
  - **Description**: The block number or one of the predefined block identifiers (such as "latest", "earliest", "pending").
  - **Required**: Yes
  - **Default/Supported Values**: "latest" (default), "earliest", "pending", or a specific block number in hexadecimal format.

### Request Example

#### API Endpoint

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```
Here’s a sample cURL request using eth_getBalance

#### Request

{% tabs %}
{% tab title="curl" %}
```json
curl --location --request POST https://go.getblock.io/<ACCESS-TOKEN>/
--header 'Content-Type: application/json' 
--data-raw {
  "jsonrpc": "2.0",
  "method": "eth_getBalance",
  "params": ["0x8D97689C9818892B700e27F316cc3E41e17fBeb9", "latest"],
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
  "result": "0x0"
}

```

### Body Parameters

Here is the list of body parameters for eth_getBalance method:

1. **jsonrpc**: Specifies the version of the JSON-RPC protocol. Typically set to "2.0".
   
2. **id**: A unique identifier for the request. It can be any string or number, in this case, "getblock.io" is used.

3. **result**: The balance of the account in hexadecimal format. In this example, "0x0" indicates a zero balance.

### Use Cases

Here are some use-cases for eth_getBalance method in Web3 programming:

1. **Account Balance Monitoring**: This method is commonly used to check the balance of an Ethereum account. Developers can use it to monitor account balances in real-time, which is essential for applications like wallets or financial dashboards. By regularly querying account balances, users can stay informed about their holdings and make timely decisions regarding transactions or investments.

2. **Transaction Validation**: Before executing a transaction, it is crucial to ensure that the sender's account has sufficient funds to cover the transaction amount and any associated gas fees. This method can be used to verify the sender's balance, preventing failed transactions due to insufficient funds. This validation step helps maintain the integrity of decentralized applications by ensuring that only feasible transactions are processed.

3. **Historical Balance Analysis**: Although typically used to fetch the latest balance, this method can also retrieve historical balances by specifying a block number. This capability is useful for applications that require analysis of an account's balance over time, such as auditing tools or financial reporting systems. By examining past balances, users can gain insights into account activity and trends.

### Code for eth_getBalance

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
  "result": "0x0"
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
When using the eth_getBalance JSON-RPC API BSC method, the following issues may occur:  
- Incorrect address format: Ensure the address is a valid hexadecimal string starting with '0x'. Double-check for any typos or incorrect characters.  
- Network syncing issues: If the node is not fully synced with the BSC network, the balance returned may be outdated. Make sure your node is fully synchronized to get the latest balance.  
- Misconfigured endpoint: If the JSON-RPC endpoint is not correctly set up, requests may fail. Verify the endpoint URL and network configuration, particularly if using a third-party provider.  
- Rate limiting by provider: Some providers impose limits on the number of requests. If you encounter rate limiting, consider optimizing your requests or upgrading your service plan.

Using the eth_getBalance method is essential for determining the balance of an address in real-time, which is crucial for managing transactions and smart contract interactions in Web3 applications. It provides a reliable way to monitor account balances, enabling developers to build responsive and efficient decentralized applications.

### conclusion

The eth_getBalance JSON-RPC method is essential for retrieving the balance of a specific Ethereum or Binance Smart Chain (BSC) address. By using eth_getBalance, developers can easily access the latest balance information to facilitate transactions and manage assets on these blockchain networks. This method is a crucial component for applications interacting with Ethereum and BSC, ensuring accurate and up-to-date financial data.
