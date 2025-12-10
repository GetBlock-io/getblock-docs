---
description: >-
  Example code for the /v1 JSON-RPC method. Сomplete guide on how to use /v1
  json-rpc in GetBlock.io Web3 documentation.
---

# /v1 - Aptos

This endpoint retrieves the latest information from the Aptos blockchain ledger, such as current chain ID, node role, latest and oldest ledger versions, epoch, block height, and more.\
In short, it provides an overview of Aptos and its health status.

## Supported Network

* Mainnet

***

## Parameters

* None

## Request

**URL (endpoint)**

```bash
https://go.getblock.io/
```

**Example (cURL)**

```bash
curl --location 'https://go.getblock.io/<ACCESS_TOKEN>/v1/info'
```

## Response

```json
{
  "block_height": "58851650",
  "chain_id": 1,
  "epoch": "2796",
  "git_hash": "6568c5ee6a58b4f96c0780d4f66d7e573e61c418",
  "ledger_timestamp": "1685696086534090",
  "ledger_version": "152087593",
  "node_role": "full_node",
  "oldest_block_height": "1101178",
  "oldest_ledger_version": "2287593"
}
```

## Response Definition

| Value                   | Data type | Description                                            |
| ----------------------- | --------- | ------------------------------------------------------ |
| `chain_id`              | integer   | ID of the blockchain                                   |
| `epoch`                 | string    | Current epoch number                                   |
| `ledger_version`        | string    | The latest version of the ledger                       |
| `oldest_ledger_version` | string    | The earliest version of the ledger                     |
| `ledger_timestamp`      | string    | Timestamp of the latest ledger version in microseconds |
| `node_role`             | string    | Role of this node in the network                       |
| `oldest_block_height`   | string    | Lowest block height available                          |
| `block_height`          | string    | Current block height                                   |
| `git_hash`              | string    | Git hash of the node build                             |

## Use Case

The `/v1/info` endpoint is useful for checking if a node is online and responding.\
For example:

* Developers can verify the latest block or ledger version to ensure the node is up-to-date.
* Applications can notify users about the health of the blockchain, such as showing a modal alerting them to the current state when they log in to a dApp.

## Code Example (Axios)

```js
import axios from 'axios'

let config = {
  method: 'get',
  maxBodyLength: Infinity,
  url: 'https://go.getblock.io/<ACCESS_TOKEN>/v1/info',
  headers: {}
};

axios.request(config)
.then((response) => {
  console.log(JSON.stringify(response.data));
})
.catch((error) => {
  console.log(error);
});
```

> Replace `<ACCESS_TOKEN>` with your actual access token from GetBlock.

## Error Handling

When using this endpoint, you may encounter:

* **404** → Incorrect or incomplete access token.
* **500** → Network issues.

**How to fix**:

* Use a stable and strong network connection.
* Check or re-copy your access token.
* Verify the URL to ensure it is correct and complete.

## Integration

It is recommended to use this endpoint first to check the current status of the blockchain before integrating other endpoints into your application.
