---
description: >-
  Use eth_call in the JSON-RPC API Interface to execute smart contract code on the BSC network without making a state change.
---

# eth_call

{% hint style="success" %}
The RPC eth_call for BSC executes a smart contract function without creating a transaction, allowing data retrieval from the blockchain without state changes.&#x20;
{% endhint %}

The eth_call Web3 method in the BSC protocol allows users to execute smart contract code locally without altering the blockchain's state. It's a read-only operation used primarily for retrieving data from contracts, such as balances or token details, without incurring gas costs. This method requires specifying a transaction call object, including the contract address and optional parameters like gas and value. The eth_call RPC protocol is essential for developers needing to simulate contract interactions or debug smart contracts without affecting the network. Efficient and precise, it ensures accurate data retrieval while maintaining the network's integrity.

### Supported Networks

The eth_call REST API method supports the following network types
- **Mainnet**
- **Testnets**

### Parameters

Here is the list of parameters eth_call method needs to be executed:

- **Parameter 1:**
  - **Type:** Object
  - **Description:** The call object containing details about the transaction to be executed.
  - **Details:**
    - **to** (required): The address of the contract to interact with.
      - **Type:** String
      - **Description:** The destination address for the transaction, typically a smart contract.
    - **data** (optional): The data to send with the transaction, usually the function selector and arguments.
      - **Type:** String
      - **Description:** Encoded function call and parameters to be executed on the contract.

- **Parameter 2:**
  - **Type:** String
  - **Description:** The block number, or a string representing the state of the blockchain to execute the call against.
  - **Default/Supported Values:** "latest", "earliest", "pending", or a specific block number.

- **Parameter 3:**
  - **Type:** Object
  - **Description:** A map of contract addresses to their corresponding contract code, allowing for execution in a simulated environment.
  - **Details:**
    - **Key** (required): Contract address.
      - **Type:** String
      - **Description:** The address of the contract to be simulated.
    - **Value** (required): Contract code.
      - **Type:** Object
      - **Description:** Contains the bytecode of the contract to be used in the simulation.
      - **Details:**
        - **code** (required): The bytecode of the contract.
          - **Type:** String
          - **Description:** The compiled bytecode of the smart contract to be used in the call simulation.

### Request Example

#### API Endpoint

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```
Here’s a sample cURL request using eth_call

#### Request

{% tabs %}
{% tab title="curl" %}
```json
curl --location --request POST https://go.getblock.io/<ACCESS-TOKEN>/
--header 'Content-Type: application/json' 
--data-raw {
  "jsonrpc": "2.0",
  "method": "eth_call",
  "params": [
    {
      "to": "0x0fd43c8fabe26d70dfa4c8b6fa680db39f147460",
      "data": "0x919840ad"
    },
    "latest",
    {
      "0x0fd43c8fabe26d70dfa4c8b6fa680db39f147460": {
        "code": "0x6080604052348015600f57600080fd5b506004361060285760003560e01c8063919840ad14602d575b600080fd5b60336045565b60408051918252519081900360200190f35b60005a90509056fea265627a7a72315820df124583906aafd283490b866399b6762e2075e1d84214363893c5993a13276f64736f6c63430005110032"
      }
    }
  ],
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
  "result": "0x00000000000000000000000000000000000000000000000000000000017d2582"
}

```

### Body Parameters

Here is the list of body parameters for eth_call method:

1. **jsonrpc**: This indicates the version of the JSON-RPC protocol being used. In this case, it's "2.0".

2. **id**: A unique identifier for the request. It can be any string or number that the client uses to match the response with the request. Here, it is "getblock.io".

3. **result**: The outcome of the eth_call method, which is typically the returned data from the executed call. In this example, it is "0x00000000000000000000000000000000000000000000000000000000017d2582".

### Use Cases

Here are some use-cases for eth_call method in Web3 programming:

1. **Reading Contract State**: One of the primary uses is to read data from a smart contract without changing the blockchain state. For example, you can use it to check the balance of an ERC-20 token for a specific address or to retrieve the current owner of a contract. This is useful for applications that need to display contract data to users without incurring gas costs.

2. **Simulating Transactions**: Before sending a transaction that changes the state, developers often simulate the transaction using this method to ensure it will succeed. This helps in identifying potential errors or reverts in the transaction logic without spending any gas, providing a safe way to test transactions.

3. **Fetching Contract Metadata**: It can be used to call functions that return metadata about a contract, such as its version, name, or supported interfaces. This is particularly useful for decentralized applications that interact with various contracts and need to verify compatibility or functionality before proceeding with further operations.

### Code for eth_call

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
  "result": "0x00000000000000000000000000000000000000000000000000000000017d2582"
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
When using the eth_call JSON-RPC API BSC method, the following issues may occur:  
- Invalid contract address: Ensure the 'to' field contains a valid, checksummed Ethereum address to prevent errors.  
- Incorrect data payload: Verify that the 'data' field contains the correct function selector and parameters encoded in hexadecimal format to ensure the function is called correctly.  
- Outdated block reference: Using an incorrect block identifier, such as an old block number, can lead to unexpected results. Always specify 'latest' to get the most recent state of the blockchain.  
- Contract code not found: If the contract code is missing or incorrectly specified in the 'params', the call will fail. Make sure the contract is deployed and the code is correctly provided.

Using the eth_call method in Web3 applications allows developers to interact with smart contracts in a read-only manner, which is crucial for retrieving data without modifying the blockchain state. This method enables efficient querying of contract states and helps in building responsive and data-driven applications.

### conclusion

The eth_call method in JSON-RPC is a crucial tool for interacting with smart contracts on blockchain networks like Ethereum and BSC. It allows users to execute a read-only call, retrieving data without altering the blockchain state. This functionality is essential for developers and users who need to access contract information efficiently and securely.
