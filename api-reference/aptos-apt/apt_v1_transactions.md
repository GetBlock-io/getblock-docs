---
description: >-
  Example code for the /v1/transactions json-rpc method. Сomplete guide on how
  to use /v1/transactions json-rpc in GetBlock.io Web3 documentation.
---

# /v1/transactions - Aptos

This endpoint gets the list of recent transactions from the Aptos blockchain.

## Supported Network
- Mainnet 

## Parameter

<table>
  <tr>
   <td><strong>Parameter</strong>
   </td>
   <td><strong>Type</strong>
   </td>
   <td><strong>In</strong>
   </td>
   <td><strong>Required</strong>
   </td>
   <td><strong>Description</strong>
   </td>
  </tr>
  <tr>
   <td>start
   </td>
   <td>Integer
   </td>
   <td>Query
   </td>
   <td>No
   </td>
   <td>Starting transaction version for pagination.
   </td>
  </tr>
  <tr>
   <td>limit
   </td>
   <td>Integer
   </td>
   <td>Query
   </td>
   <td>No
   </td>
   <td>Number of transactions to return. Default: 25. Maximum: 100.
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
curl --location 'https://go.getblock.io/<ACCESS_TOKEN>/v1/transactions?limit=1'
```

## Response Example

```json
[
    {
        "version": "3363892352",
        "hash": "0x5b34f4618d0e421f46b2d0a07ba307caabc17c5d28b20e04a2436394187c0272",
        "state_change_hash": "0xafb6e14fe47d850fd0a7395bcfb997ffacf4715e0f895cc162c218e4a7564bc6",
        "event_root_hash": "0x414343554d554c41544f525f504c414345484f4c4445525f4841534800000000",
        "state_checkpoint_hash": "0x5267087614437f5aae0662342f49b8e06756c3d74c994af195a6e9a2976f29e0",
        "gas_used": "0",
        "success": true,
        "vm_status": "Executed successfully",
        "accumulator_root_hash": "0x12aa45cc9a90fc25804d7b5017b7aecff53ce7f0fa376ef3c9f9b9e5bb781e97",
        "changes": [],
        "timestamp": "1757261604675388",
        "block_end_info": {
            "block_gas_limit_reached": false,
            "block_output_limit_reached": false,
            "block_effective_block_gas_units": 135,
            "block_approx_output_size": 20765
        },
        "type": "block_epilogue_transaction"
    }
]

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
   <td>Global transaction version (unique ID in Aptos ledger).
   </td>
  </tr>
  <tr>
   <td>hash
   </td>
   <td>String
   </td>
   <td>Transaction hash.
   </td>
  </tr>
  <tr>
   <td>state_change_hash
   </td>
   <td>String
   </td>
   <td>Hash of all state changes caused by this transaction.
   </td>
  </tr>
  <tr>
   <td>event_root_hash
   </td>
   <td>String
   </td>
   <td>Merkle root hash of all events emitted.
   </td>
  </tr>
  <tr>
   <td>state_checkpoint_hash
   </td>
   <td>String
   </td>
   <td>Hash of the state checkpoint for this transaction.
   </td>
  </tr>
  <tr>
   <td>gas_used
   </td>
   <td>String
   </td>
   <td>Amount of gas consumed (0 here, since it’s a system txn).
   </td>
  </tr>
  <tr>
   <td>success
   </td>
   <td>Boolean
   </td>
   <td>Whether the transaction executed successfully.
   </td>
  </tr>
  <tr>
   <td>vm_status
   </td>
   <td>String
   </td>
   <td>VM execution result.
   </td>
  </tr>
  <tr>
   <td>accumulator_root_hash
   </td>
   <td>String
   </td>
   <td>Ledger’s global accumulator root after applying this transaction.
   </td>
  </tr>
  <tr>
   <td>changes
   </td>
   <td>Array
   </td>
   <td>State changes caused (empty here).
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
   <td>Metadata about block execution limits.
   </td>
  </tr>
  <tr>
   <td>block_end_info.block_gas_limit_reached
   </td>
   <td>Boolean
   </td>
   <td>Whether the block’s gas limit was hit.
   </td>
  </tr>
  <tr>
   <td>block_end_info.block_output_limit_reached
   </td>
   <td>Boolean
   </td>
   <td>Whether block output size was exceeded.
   </td>
  </tr>
  <tr>
   <td>block_end_info.block_effective_block_gas_units
   </td>
   <td>Interger
   </td>
   <td>Total effective gas used in the block.
   </td>
  </tr>
  <tr>
   <td>block_end_info.block_approx_output_size
   </td>
   <td>Integer
   </td>
   <td>Approximate output size of the block in bytes.
   </td>
  </tr>
  <tr>
   <td>type
   </td>
   <td>String
   </td>
   <td>Transaction type → here it’s block_epilogue_transaction (special system txn marking block end).
   </td>
  </tr>
</table>

## Use Cases

This method is used for:
* Display a feed of recent blockchain activity.
* Monitor all user-submitted transactions in real time.
* Build analytics dashboards for transaction patterns.


## Code Example

**Node(Axios)**


```
const axios = require('axios');

let config = {
  method: 'get',
  maxBodyLength: Infinity,
  url: 'https://go.getblock.io/<ACCESS_TOKEN>/v1/transactions?limit=1',
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

url = "https://go.getblock.io/<ACCESS_TOKEN>/v1/transactions?limit=1"

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
   <td>Forbidden
   </td>
   <td>Missing or invalid ACCESS_TOKEN.
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

By integrating `/v1/transactions`, developers can:

* Stream blockchain activity into wallets and dApps.

* Enable dashboards showing the latest transfers, mints, or contract calls.

* Support DeFi/NFT protocols by tracking relevant transactions.

* Provide analytics & insights on transaction frequency, volume, and patterns.
