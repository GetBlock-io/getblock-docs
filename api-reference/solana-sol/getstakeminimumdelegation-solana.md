---
description: >-
  The getStakeMinimumDelegation JSON-RPC method returns the stake minimum
  delegation required in lamports.
---

# getStakeMinimumDelegation – Solana

{% hint style="success" %}
The getStakeMinimumDelegation RPC Solana method provides the minimum stake amount necessary to delegate stake to a validator.&#x20;
{% endhint %}

This value is denominated in lamports, Solana's smallest currency unit. The method supports an optional parameters object to specify commitment levels for data accuracy.

The Core API uses this method to help validators, dApps, and wallet applications manage stake operations effectively.

### Supported Networks

This method is available on the following API endpoints:

* Mainnet
* Devnet

### Parameters

#### Optional Parameters

* object (optional): A configuration object containing:
  * commitment (string, optional): Defines the level of finality for the request.

### Result

The response returns an RpcResponse object containing:

* context (object): Provides contextual information about the slot in which the value was retrieved.
  * slot (u64): The slot number of the data.
* value (u64): The stake minimum delegation amount in lamports.

### Request Example

#### API Endpoints

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

#### cURL Example

{% tabs %}
{% tab title="curl" %}
```
curl --location "https://go.getblock.io/api-key" -XPOST \
--header "Content-Type: application/json" \
--data '{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "getStakeMinimumDelegation"
}'
```
{% endtab %}
{% endtabs %}

### Response

A successful request returns the minimum stake delegation value.

#### Example Response

```json
{
  "jsonrpc": "2.0",
  "result": {
    "context": {
      "slot": 501
    },
    "value": 1000000000
  },
  "id": 1
}
```

In this response, 1,000,000,000 lamports (equivalent to 1 SOL) is the minimum stake delegation.

### Error Handling

Common getStakeMinimumDelegation error scenarios:

* Invalid parameters: If the request contains malformed parameters.
* Network errors: Connectivity issues with the Solana JSON-RPC API endpoints.

#### Example Error Response

```json
{
  "jsonrpc": "2.0",
  "error": {
    "code": -32602,
    "message": "Invalid parameter format"
  },
  "id": 1
}
```

### Use Cases

The Solana getStakeMinimumDelegation method is useful for:

* Validators and node operators: Understanding the minimum stake required for delegation.
* Wallet applications: Displaying the minimum stake requirement to users.
* Web3 analytics tools: Analyzing staking activity and requirements.

### Code Example – Web3 getStakeMinimumDelegation Integration

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

const url = "https://go.getblock.io/api-key"; 
const headers = { "Content-Type": "application/json" };

const payload = {
  jsonrpc: "2.0",
  id: 1,
  method: "getStakeMinimumDelegation"
};

const fetchStakeMinimumDelegation = async () => {
  try {
    const response = await axios.post(url, payload, { headers });

    if (response.status === 200 && response.data.result?.value !== undefined) {
      const minDelegation = response.data.result.value;
      console.log("Minimum Stake Delegation (lamports):", minDelegation);
    } else {
      console.error("Unexpected response:", response.data);
    }
  } catch (error) {
    console.error("getStakeMinimumDelegation error:", error.response?.data || error.message);
  }
};

fetchStakeMinimumDelegation();

```
{% endtab %}
{% endtabs %}

### Integration with Web3

By integrating getStakeMinimumDelegation into Solana’s Core API, developers can manage staking operations, query the blockchain for current transaction requirements, and provide stake-related information to users efficiently. This JSON-RPC method ensures accurate and up-to-date insights into stake requirements for validators and delegators.

\
