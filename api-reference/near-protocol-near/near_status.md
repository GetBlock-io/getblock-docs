---
description: >-
  Example code for the status json-rpc method. Сomplete guide on how to use
  status json-rpc in GetBlock.io Web3 documentation.
---

# status - NEAR Protocol

This method returns **real-time information** about the NEAR node and the network it’s connected to.
It provides details such as the **latest block**, **validator information**, **sync status**, and **node version** — making it essential for **monitoring node health** and **network connectivity**.

## Supported Networks

- Mainnet

## Parameters

- None
  
## Request Example

**Base URL**

```bash
 https://go.getblock.io/<ACCESS_TOKEN>
```
**Example(cURL)**

  ```curl
  curl -X POST https://go.getblock.io/<ACCESS_TOKEN> \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc": "2.0",
  "method": "status",
  "params": [],
  "id": "getblock.io"
  }'
  ```

## Response Example

```json
{
    "jsonrpc": "2.0",
    "result": {
        "chain_id": "mainnet",
        "genesis_hash": "EPnLgE7iEq9s7yTkos96M3cWymH5avBAPm3qx3NXqR8H",
        "latest_protocol_version": 81,
        "node_key": null,
        "node_public_key": "ed25519:25NKtJin6cs4dDVqfrX15F8qHL8APKEPsZ8ZXkeQcZ1h",
        "protocol_version": 80,
        "rpc_addr": "0.0.0.0:3030",
        "sync_info": {
            "earliest_block_hash": "2YTH12o4WD5z8uMSNim1oM45oRZMMJVnHXYRbEGwXoL9",
            "earliest_block_height": 169582361,
            "earliest_block_time": "2025-10-24T00:26:02.397361265Z",
            "epoch_id": "78JmDEzNWqu1Z2RmLKoVqNvRn8tedtb1CwgyWsVg3ZK7",
            "epoch_start_height": 169789917,
            "latest_block_hash": "HPAkpQHzUJdNJm2UvguyGhwQHucyq47vACSfiUqNYTFu",
            "latest_block_height": 169796979,
            "latest_block_time": "2025-10-25T12:36:34.343644643Z",
            "latest_state_root": "43XupNhD4YyV9BkguKzBEqFzk8KKvhWMozaoWMiNPP8q",
            "syncing": false
        },
        "uptime_sec": 424867,
        "validator_account_id": null,
        "validator_public_key": null,
        "validators": [
            {
                "account_id": "bisontrails2.poolv1.near"
            },
            {
                "account_id": "astro-stakers.poolv1.near"
            },
            {
                "account_id": "zavodil.poolv1.near"
            },
            {
                "account_id": "binancenode1.poolv1.near"
            },
            {
                "account_id": "bitwise_1.poolv1.near"
            },
            {
                "account_id": "ledgerbyfigment.poolv1.near"
            },
        ],
        "version": {
            "build": "unknown",
            "commit": "unknown",
            "rustc_version": "1.86.0",
            "version": "2.9.0"
        }
    },
    "id": "getblock.io"
}
```

## Response Parameters Definition

| **Field**                           | **Type** | **Description**                                                               |
| ----------------------------------- | -------- | ----------------------------------------------------------------------------- |
| **chain_id**                        | string   | The unique identifier of the blockchain network (e.g., `mainnet`, `testnet`). |
| **latest_protocol_version**         | integer  | The latest protocol version supported by the network.                         |
| **node_key**                        | string   | The node’s cryptographic key used for synchronisation.                        |
| **protocol_version**                | integer  | The current protocol version running on the node.                             |
| **rpc_addr**                        | string   | The RPC address through which the node communicates.                          |
| **sync_info**                       | object   | Contains synchronisation details for block processing.                        |
| **sync_info.earliest_block_hash**   | string   | Hash of the earliest block stored on the node.                                |
| **sync_info.earliest_block_height** | integer  | Height (index) of the earliest stored block.                                  |
| **sync_info.earliest_block_time**   | string   | Timestamp of the earliest block (ISO 8601 format).                            |
| **sync_info.latest_block_hash**     | string   | Hash of the most recent block processed by the node.                          |
| **sync_info.latest_block_height**   | integer  | Height of the most recent block.                                              |
| **sync_info.latest_block_time**     | string   | Timestamp of the most recent block (ISO 8601 format).                         |
| **sync_info.latest_state_root**     | string   | State root hash of the most recent block.                                     |
| **sync_info.syncing**               | boolean  | Indicates whether the node is still syncing with the network.                 |
| **uptime_sec**                      | integer  | Node uptime in seconds.                                                       |
| **validator_account_id**            | string   | Account ID of the validator operating this node.                              |
| **validators**                      | array    | List of validators currently active in the network.                           |
| **validators.account_id**           | string   | Identifier of the validator’s account.                                        |
| **validators.is_slashed**           | boolean  | Shows if the validator has been slashed for misbehaviour.                      |
| **version**                         | object   | Contains node software version details.                                       |
| **version.version**                 | string   | Node software version number.                                                 |
| **version.build**                   | string   | Build label or release type (e.g., `release`, `debug`).                       |
| **version.rustc_version**           | string   | Rust compiler version used to build the node.                                 |


## Use Cases

* Monitor **node health** and connection status.
* Retrieve **network-level information** for dashboards.
* Detect if a node is **out of sync** or lagging behind the network.
* Display **protocol and software version** information for maintenance and debugging.

## Code Example

**Node(Axios)**
```js
import axios from 'axios';
let data = JSON.stringify({
  "jsonrpc": "2.0",
  "method": "status",
  "params": [],
  "id": "getblock.io"
});

let config = {
  method: 'post',
  maxBodyLength: Infinity,
  url: 'https://go.getblock.io/<ACCESS_TOKEN>',
  headers: { 
    'Content-Type': 'application/json'
  },
  data : data
};

axios.request(config)
.then((response) => {
  console.log(JSON.stringify(response.data));
})
.catch((error) => {
  console.log(error);
});
```

**Python(Requests)**

```python
import requests
import json

url = "https://go.getblock.io/<ACCESS_TOKEN>"

payload = json.dumps({
  "jsonrpc": "2.0",
  "method": "status",
  "params": [],
  "id": "getblock.io"
})
headers = {
  'Content-Type': 'application/json'
}

response = requests.request("POST", url, headers=headers, data=payload)
print(response.text)
```

## Error Handling

| **HTTP Code**                 | **Error Message**              | **Description**                                        |
| ----------------------------- | ------------------------------ | ------------------------------------------------------ |
| **500 Internal Server Error** | `Node processing error`        | The node encountered an issue while retrieving status. |
| **403 Forbidden**   | `RBAC: access denied` | The getblock access token is missing or incorrect                |


## Integration with Web3
By integrating `/status` into dApp, developers can:

- Build **network status dashboards** for validators and node operators.
- Enable **automatic node monitoring** and alerts.
- Check **protocol upgrades** or changes in real-time.
