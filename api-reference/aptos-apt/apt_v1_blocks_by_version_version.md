---
description: >-
  Example code for the /v1/blocks/by_version/{version} JSON-RPC method. Сomplete
  guide on how to use /v1/blocks/by_version/{version} json-rpc in GetBlock.io
  Web3 documentation.
---

# /v1/transactions/by_version/{version}

This endpoint gets a transaction by its ledger version number from Aptos blockchain.

## Supported Network

* Mainnet
  
## Parameter

<table>
  <tr>
   <td><strong>Parameter</strong>
   </td>
   <td><strong>Data type</strong>
   </td>
   <td><strong>Description</strong>
   </td>
   <td><strong>Required</strong>
   </td>
   <td><strong>In</strong>
   </td>
  </tr>
  <tr>
   <td>version
   </td>
   <td>integer
   </td>
   <td>The ledger version 
   </td>
   <td>Yes
   </td>
   <td>Path
   </td>
  </tr>
    <tr>
   <td>with_transactions
   </td>
   <td>boolean
   </td>
   <td>To contain transactions' details or not
   </td>
   <td>no
   </td>
   <td>Query
   </td>
  </tr>
</table>



## Request

**Base URL**
```bash
https://go.getblock.io/<ACCESS_TOKEN>
```
**Example(cURL)**
```curl
curl --location 'https://go.getblock.io/<ACCESS_TOKEN>/v1/transactions/by_version/3556737308?with_transactions=false'
```

## Response Example

```json
{
    "block_height": "456015808",
    "block_hash": "0xdbe2cbd48ec3b897fa5f6b1ff51bd8eef280e300e12b6ff153b3959a7440a268",
    "block_timestamp": "1760341227055115",
    "first_version": "3556737303",
    "last_version": "3556737309",
    "transactions": null
}

```
## Response Parameter Definition

| Field                                      | Type                             | Description                                                                       |
| ------------------------------------------ | -------------------------------- | --------------------------------------------------------------------------------- |
| **block_height**                           | string                  | Height of the block in the blockchain.                                            |
| **block_hash**                             | string                 | Unique hash identifying the block.                                                |
| **block_timestamp**                        | string                  | Timestamp (in microseconds) when the block was created.                           |
| **first_version**                          | string                 | First transaction version included in this block.                                 |
| **last_version**                           | string                 | Last transaction version included in this block.                                  |
| **transactions**                           | objects         | List of transactions contained in this block.                                     |
| **transactions.type**                      | string                   | Type of the transaction (e.g., `user_transaction`, `block_metadata_transaction`). |
| **transactions.hash**                      | string                  | Unique hash identifying the transaction.                                          |
| **transactions.sender**                    | string                 | Account address that initiated the transaction.                                   |
| **transactions.sequence_number**           | string                    | The sender’s sequence number for this transaction.                                |
| **transactions.max_gas_amount**            | string                  | Maximum gas units the sender is willing to spend.                                 |
| **transactions.gas_unit_price**            | string                  | Gas price per unit.                                                               |
| **transactions.expiration_timestamp_secs** | string                   | Expiration timestamp (in seconds) after which the transaction becomes invalid.    |
| **transactions.payload**                   | object                  | Payload object describing the action being executed.                              |
| **transactions.payload.type**              | string                | Type of payload (e.g., `entry_function_payload`).                                 |
| **transactions.payload.function**          | string                   | Function name being called (e.g., `0x1::coin::transfer`).                         |
| **transactions.payload.type_arguments**    | array       | Type arguments for the function (e.g., token types).                              |
| **transactions.payload.arguments**         | array | Arguments passed to the function (e.g., recipient address, amount).               |
| **transactions.signature**                 | object                 | Signature object verifying the transaction.                                       |
| **transactions.signature.type**            | string                 | Type of cryptographic signature (e.g., `ed25519_signature`).                      |
| **transactions.signature.public_key**      | string                | Public key of the sender.                                                         |
| **transactions.signature.signature**       | string                 | Cryptographic signature validating the transaction.                               |




## Use cases

This method is used for:
* Retrieve details of a specific transaction by its version.
* Debug __failed transactions__ by checking vm_status.
* Build __explorers__ that let users search transactions by version.
* Track __system-level transactions__ like block epilogues.


## Code Example

**Node(Axios)**

```js
const axios = require('axios');

let config = {
  method: 'get',
  maxBodyLength: Infinity,
  url: 'https://go.getblock.io/<ACCESS_TOKEN>/v1/transactions/by_version/3363904007',
};

axios.request(config)
.then((response) => {
  console.log(JSON.stringify(response.data));
})
.catch((error) => {
  console.log(error);
});
```

**Python(Request)**

```python
import requests

url = 'https://go.getblock.io/<ACCESS_TOKEN>/v1/transactions/by_version/3363904007'

response = requests.request("GET", url, headers=headers, data=payload)

print(response.text)
```

## Error handling


<table>
  <tr>
   <td><strong>Status Code</strong>
   </td>
   <td><strong>Error Message</strong>
   </td>
   <td><strong>Cause</strong>
   </td>
  </tr>
  <tr>
   <td><strong>403</strong>
   </td>
   <td>forbidden
   </td>
   <td>Missing or invalid &lt;ACCESS_TOKEN>.
   </td>
  </tr>
  <tr>
   <td><strong>410</strong>
   </td>
   <td>Ledger version has been pruned
   </td>
   <td>Incorrect version number or being pruned
   </td>
  </tr>
  <tr>
   <td><strong>500</strong>
   </td>
   <td>Internal server error
   </td>
   <td>Node or network issue; retry later.
   </td>
  </tr>
</table>



## Integration with Web3

By integrating `/v1/transactions/by_version/{version}`, developers can:

* __Trace exact transactions__ for auditing or compliance.
* __Debug dApps__ by fetching execution results and state changes.
* __Enable explorers__ to link transactions with block details. \
__Monitor validators/system txns__ like block prologues and epilogues.
