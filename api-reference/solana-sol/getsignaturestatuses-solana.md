---
description: >-
  The getSignatureStatuses JSON-RPC method retrieves the status of one or more
  transaction signatures in the Solana blockchain.
---

# getSignatureStatuses – Solana

{% hint style="success" %}
The getSignatureStatuses RPC Solana method checks the status of a list of transaction signatures (txid).&#x20;
{% endhint %}

By default, it only searches the recent status cache, which includes all active slots plus MAX\_RECENT\_BLOCKHASHES rooted slots. However, enabling the searchTransactionHistory parameter allows a deeper search in the node's ledger.

This method is valuable for applications that need to verify whether a block contains a given transaction and determine its confirmation level using Solana’s Core API.

### Supported Networks

This method is available on the following API endpoints:

* Mainnet
* Devnet

### Parameters

#### Required Parameters

* array (required): An array of transaction signatures, provided as base-58 encoded strings.
  * Maximum: 256 signatures.

#### Optional Parameters

* object (optional): A configuration object containing:
  * searchTransactionHistory (bool, optional): If true, searches the node's ledger for signatures not found in the recent status cache.

### Result

The response returns an array of RpcResponse objects, each containing:

* null: If the transaction is unknown.
* object:
  * slot (u64): The slot in which the transaction was processed.
  * confirmations (usize|null): The number of blocks since the transaction was confirmed.
    * null if finalized by the supermajority.
  * err (object|null): If the transaction failed, contains an error object. Otherwise, null.
  * confirmationStatus (string|null): The transaction’s cluster confirmation status.
    * Possible value: processed, confirmed, finalized.
  * DEPRECATED: status (object):
    * { "Ok": null } if the transaction was successful.
    * { "Err": \<ERR> } if the transaction failed with an error.

### Request Example

#### API Endpoints

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

#### cURL Example

{% tabs %}
{% tab title="curl" %}
```json
curl --location "https://go.getblock.io/<ACCESS-TOKEN>/" -XPOST \
--header "Content-Type: application/json" \
--data '{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "getSignatureStatuses",
    "params": [
      [
        "5VERv8NMvzbJMEkV8xnrLkEaWRtSz9CosKDYjCJjBRnbJLgp8uirBgmQpjKhoR4tjF3ZpRzrFmBV6UjKdiSZkQUW"
      ],
      {
        "searchTransactionHistory": true
      }
    ]
}'
```
{% endtab %}
{% endtabs %}

### Response

A successful request returns transaction status details.

#### Example Response

```json
{
  "jsonrpc": "2.0",
  "result": {
    "context": {
      "slot": 82
    },
    "value": [
      {
        "slot": 48,
        "confirmations": null,
        "err": null,
        "status": {
          "Ok": null
        },
        "confirmationStatus": "finalized"
      },
      null
    ]
  },
  "id": 1
}
```

### Error Handling

Common getSignatureStatuses error scenarios:

* Invalid transaction signature: If the provided signature is not a valid txid.
* Network errors: Connectivity issues with the Solana JSON-RPC API endpoints.
* Malformed request: Incorrectly structured JSON requests.

#### Example Error Response

```json
{
  "jsonrpc": "2.0",
  "error": {
    "code": -32602,
    "message": "Invalid transaction signature format"
  },
  "id": 1
}
```

### Use Cases

The Solana getSignatureStatuses method is useful for:

* dApp developers: Monitoring transaction confirmations and finality.
* Web3 analytics tools: Tracking transaction status trends.
* Validators and node operators: Checking the status of processed transactions.
* Blockchain explorers: Displaying transaction history and confirmations.

### Code getSignatureStatuses Example – Web3 Integration

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

const url = "https://go.getblock.io/<ACCESS-TOKEN>/"; 
const headers = { "Content-Type": "application/json" };

const payload = {
  jsonrpc: "2.0",
  id: 1,
  method: "getSignatureStatuses",
  params: [
    [
      "5VERv8NMvzbJMEkV8xnrLkEaWRtSz9CosKDYjCJjBRnbJLgp8uirBgmQpjKhoR4tjF3ZpRzrFmBV6UjKdiSZkQUW"
    ],
    { searchTransactionHistory: true }
  ]
};

const fetchSignatureStatuses = async () => {
  try {
    const response = await axios.post(url, payload, { headers });

    if (response.status === 200 && response.data.result?.value) {
      const statuses = response.data.result.value;
      statuses.forEach((status, index) => {
        if (status) {
          console.log(`Signature ${index + 1}:`);
          console.log(`  Slot: ${status.slot}`);
          console.log(`  Confirmations: ${status.confirmations}`);
          console.log(`  Status: ${status.confirmationStatus}`);
          console.log(`  Err: ${status.err || "No error"}`);
        } else {
          console.log(`Signature ${index + 1}: Not found`);
        }
      });
    } else {
      console.error("Unexpected response:", response.data);
    }
  } catch (error) {
    console.error("getSignatureStatuses error:", error.response?.data || error.message);
  }
};

fetchSignatureStatuses();

```
{% endtab %}
{% endtabs %}

### Integration with Web3

By integrating Web3 getSignatureStatuses into Solana’s Core API, developers can verify transaction statuses in real time, track block confirmations, and ensure efficient request handling. This JSON-RPC method is essential for decentralized applications, validators, and blockchain explorers that require up-to-date transaction insights.

\
