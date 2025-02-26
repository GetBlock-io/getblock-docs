---
description: >-
  The getSignaturesForAddress JSON-RPC method retrieves signatures for confirmed
  transactions that involve a given account address in their accountKeys list.
---

# getSignaturesForAddress – Solana

{% hint style="success" %}
The getSignaturesForAddress RPC Solana method returns transaction signatures from the most recent confirmed block, moving backwards in time.
{% endhint %}

Developers can filter results using optional parameters such as limits, starting and ending signatures, and confirmation levels. This allows efficient querying of historical transaction records using Solana’s Core API.

The response provides an ordered array of transaction signatures with associated metadata such as slot, error status, memo, estimated block time, and confirmation status.

### Supported Networks

This method is available on the following API endpoints:

* Mainnet
* Devnet

### Parameters

#### Required Parameters

* string (required): The account address, provided as a base-58 encoded string.

#### Optional Parameters

* object (optional): A configuration object containing:
  * commitment (string, optional): Defines the level of finality for the query.
  * minContextSlot (number, optional): The minimum slot at which the request can be evaluated.
  * limit (number, optional, default: 1000): The maximum number of transaction signatures to return.
    * Must be between 1 and 1,000.
  * before (string, optional): Start searching backwards from this transaction signature.
  * until (string, optional): Stop searching when this transaction signature is reached (if found before reaching the limit).

### Result

The response returns an ordered array (from newest to oldest) of transaction signatures, each containing:

* signature (string): The transaction signature, base-58 encoded.
* slot (u64): The slot containing the block with the transaction.
* err (object|null): If the transaction failed, this field contains an error object; otherwise, it is null.
* memo (string|null): Any memo associated with the transaction (if present).
* blockTime (i64|null): The estimated Unix timestamp of the transaction’s processing time.
  * null if unavailable.
* confirmationStatus (string|null): The confirmation status of the transaction.
  * Possible value: processed, confirmed, finalized.

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
    "method": "getSignaturesForAddress",
    "params": [
      "Vote111111111111111111111111111111111111111",
      {
        "limit": 1
      }
    ]
}'
```
{% endtab %}
{% endtabs %}

### Response

A successful request returns an array of transaction signature details.

#### Example Response

```json
{
  "jsonrpc": "2.0",
  "result": [
    {
      "err": null,
      "memo": null,
      "signature": "5h6xBEauJ3PK6SWCZ1PGjBvj8vDdWG3KpwATGy1ARAXFSDwt8GFXM7W5Ncn16wmqokgpiKRLuS83KUxyZyv2sUYv",
      "slot": 114,
      "blockTime": null
    }
  ],
  "id": 1
}
```

### Error Handling

Common getSignaturesForAddress error scenarios:

* Invalid account address: If the provided address is not correctly formatted.
* Network errors: Connectivity issues with the Solana JSON-RPC API endpoints.
* Malformed request: Incorrectly structured JSON requests.

#### Example Error Response

```json
{
  "jsonrpc": "2.0",
  "error": {
    "code": -32602,
    "message": "Invalid account address format"
  },
  "id": 1
}
```

### Use Cases

The Solana getSignaturesForAddress method is useful for:

* dApp developers: Retrieving transaction history for user accounts.
* Web3 analytics tools: Tracking transaction patterns and account activity.
* Blockchain explorers: Displaying past transactions for a given account.
* Validators and node operators: Monitoring transaction confirmations.

### Code getSignaturesForAddress Example – Web3 Integration

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const axios = require('axios');


const url = "https://go.getblock.io/<ACCESS-TOKEN>/";
const headers = { "Content-Type": "application/json" };

const payload = {
  jsonrpc: "2.0",
  id: 1,
  method: "getSignaturesForAddress",
  params: [
    "Vote111111111111111111111111111111111111111",
    { limit: 1 }
  ]
};

const fetchSignaturesForAddress = async () => {
  try {
    const response = await axios.post(url, payload, { headers });

    if (response.status === 200 && Array.isArray(response.data.result)) {
      const signatures = response.data.result;
      if (signatures.length > 0) {
        signatures.forEach((signature, index) => {
          console.log(`Signature ${index + 1}:`);
          console.log(`  Signature: ${signature.signature}`);
          console.log(`  Slot: ${signature.slot}`);
          console.log(`  Block Time: ${signature.blockTime ? new Date(signature.blockTime * 1000) : "N/A"}`);
          console.log(`  Err: ${signature.err ? JSON.stringify(signature.err) : "No error"}`);
        });
      } else {
        console.log("No signatures found for the specified address.");
      }
    } else {
      console.error("Unexpected response:", response.data);
    }
  } catch (error) {
    console.error("getSignaturesForAddress error:", error.response?.data || error.message);
  }
};

fetchSignaturesForAddress();

```
{% endtab %}
{% endtabs %}

### Integration with Web3

By integrating Web3 getSignaturesForAddress into Solana’s Core API, developers can efficiently query transaction records linked to an address, retrieve block confirmations, and optimize request performance. This JSON-RPC method is a powerful tool for tracking transaction histories in decentralized applications and blockchain infrastructure.

\
