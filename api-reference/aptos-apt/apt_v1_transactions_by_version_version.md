---
description: >-
  Example code for the /v1/transactions/by_version/{version} json-rpc method.
  Сomplete guide on how to use /v1/transactions/by_version/{version} json-rpc in
  GetBlock.io Web3 documentation.
---

# /v1/transactions/by\_version/{version} - Aptos

This endpoint gets a transaction by its ledger version number from Aptos blockchain.


## supported Network

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
   <td><code>version</code>
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
</table>

## Request

**Base URL**

```bash
https://go.getblock.io/<ACCESS_TOKEN>
```

**Example(cURL)**

```curl
curl --location 'https://go.getblock.io/<ACCESS_TOKEN>/v1/transactions/by_version/3363904007'
```

## Response Example

```json
{
    "version": "3363904007",
    "hash": "0xd797b944ed8657406a1b09a5928048093399fc0a2f576d3e57c0f9cedbf95c4a",
    "state_change_hash": "0xafb6e14fe47d850fd0a7395bcfb997ffacf4715e0f895cc162c218e4a7564bc6",
    "event_root_hash": "0x414343554d554c41544f525f504c414345484f4c4445525f4841534800000000",
    "state_checkpoint_hash": "0xbf257d934f991d1e7d74079cc9060d3ac005aaa208d93a1fd4928dfc224bba53",
    "gas_used": "0",
    "success": true,
    "vm_status": "Executed successfully",
    "accumulator_root_hash": "0xc985314d5c118899c649b772c237178c63f328e7d286aceea01a30373d491d95",
    "changes": [],
    "timestamp": "1757261767411156",
    "block_end_info": {
        "block_gas_limit_reached": false,
        "block_output_limit_reached": false,
        "block_effective_block_gas_units": 500,
        "block_approx_output_size": 21541
    },
    "type": "block_epilogue_transaction"
}

```



## Response Parameter Definition

<table>
  <tr>
   <td><strong>Field</strong>
   </td>
   <td><strong>Type</strong>
   </td>
   <td><strong>Description</strong>
   </td>
  </tr>
  <tr>
   <td>version
   </td>
   <td>String
   </td>
   <td>Global transaction version number.
   </td>
  </tr>
  <tr>
   <td>hash
   </td>
   <td>String
   </td>
   <td>Hash of the transaction.
   </td>
  </tr>
  <tr>
   <td>state_change_hash
   </td>
   <td>String
   </td>
   <td>Hash of all state changes from this txn.
   </td>
  </tr>
  <tr>
   <td>event_root_hash
   </td>
   <td>String
   </td>
   <td>Merkle root hash of all events.
   </td>
  </tr>
  <tr>
   <td>state_checkpoint_hash
   </td>
   <td>String
   </td>
   <td>Hash of the state checkpoint.
   </td>
  </tr>
  <tr>
   <td>gas_used
   </td>
   <td>String
   </td>
   <td>Amount of gas consumed.
   </td>
  </tr>
  <tr>
   <td>success
   </td>
   <td>Boolean
   </td>
   <td>Whether transaction executed successfully.
   </td>
  </tr>
  <tr>
   <td>vm_status
   </td>
   <td>String
   </td>
   <td>Result status from Aptos VM.
   </td>
  </tr>
  <tr>
   <td>accumulator_root_hash
   </td>
   <td>String
   </td>
   <td>Global accumulator root hash.
   </td>
  </tr>
  <tr>
   <td>changes
   </td>
   <td>Array
   </td>
   <td>State changes applied (empty for system txns).
   </td>
  </tr>
  <tr>
   <td>timestamp
   </td>
   <td>String
   </td>
   <td>Unix timestamp in microseconds.
   </td>
  </tr>
  <tr>
   <td>block_end_info
   </td>
   <td>Object
   </td>
   <td>Metadata about block execution.
   </td>
  </tr>
  <tr>
   <td>block_end_info.block_gas_limit_reached
   </td>
   <td>Booleab
   </td>
   <td>Block’s gas limit was reached.
   </td>
  </tr>
  <tr>
   <td>block_end_info.block_output_limit_reached
   </td>
   <td>Boolean
   </td>
   <td>Block’s output size limit was hit.
   </td>
  </tr>
  <tr>
   <td>block_end_info.block_effective_block_gas_units
   </td>
   <td>Integer
   </td>
   <td>Effective gas used in this block.
   </td>
  </tr>
  <tr>
   <td>block_end_info.block_approx_output_size
   </td>
   <td>Interger
   </td>
   <td>Approximate output size of block in bytes.
   </td>
  </tr>
  <tr>
   <td>type
   </td>
   <td>String
   </td>
   <td>Transaction type (e.g., user_transaction, block_epilogue_transaction).
   </td>
  </tr>
</table>


## Use cases

This method is used for:

* Retrieve details of a specific transaction by its version.
* Debug **failed transactions** by checking vm_status.
* Build **explorers** that let users search transactions by version.
* Track **system-level transactions** like block epilogues.


## Code Example

**Node(axios)**

```js
const axios = require('axios');

let config = {
  method: 'get',
  maxBodyLength: Infinity,
  url: 'https://go.getblock.io/<ACCESS_TOKEN>/v1/transactions/by_version/3541136893',
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

url = "https://go.getblock.io/<ACCESS_TOKEN>/v1/transactions/by_version/3541136893"

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

* **Trace exact transactions** for auditing or compliance.
* **Debug dApps** by fetching execution results and state changes.
* **Enable explorers** to link transactions with block details. 
**Monitor validators/system txns** like block prologues and epilogues.
