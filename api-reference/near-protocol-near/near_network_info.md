---
description: >-
  Example code for the network_info json-rpc method. Сomplete guide on how to
  use network_info json-rpc in GetBlock.io Web3 documentation.
---

# network\_info - NEAR Protocol

The method  provides detailed information about the **current state of the NEAR network**, including **node identity**, **peer connections**, and **active protocol version**.

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

```bash
  curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
  --header 'Content-Type: application/json' \ 
  --data-raw '{"jsonrpc": "2.0",
  "method": "network_info",
  "params": [],
  "id": "getblock.io"}'
```

## Response
```json
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "active_peers": [
            {
                "account_id": null,
                "addr": "34.91.52.248:24567",
                "id": "ed25519:8MvmovbbJ4PYNgMCpBwvQRA2P4ZbpEcZGHX1448G3Ehm"
            },
            {
                "account_id": null,
                "addr": "54.179.201.180:24567",
                "id": "ed25519:7rN5RBbgEu6c778zdX4aEqWCofgjzSC9wYzi61Pcr2AL"
            },
            {
                "account_id": null,
                "addr": "185.209.177.15:24567",
                "id": "ed25519:8rhSRLCrzPCS4JsgHFadgWGKvKCPCweq5oH3eL6iH6SK"
            }
        ],
        "known_producers": [
            {
                "account_id": "chelovek_iz_naroda.poolv1.near",
                "addr": null,
                "peer_id": "ed25519:GyuMR3KYDMVQZVH87aZqHSRgqsD1ZTsRMN8JZj1gCa2P"
            },
            {
                "account_id": "incrypted.poolv1.near",
                "addr": null,
                "peer_id": "ed25519:Hp74AobcFkyKqDU7NZQAhqbvHTesouRCveCFWfEe6NAp"
            },
            {
                "account_id": "dexagon.poolv1.near",
                "addr": null,
                "peer_id": "ed25519:6v3juNRvjGSDoauKNzQJunHv2ktPdEMj5s6zoZ6NEnqJ"
            }
        ],
        "num_active_peers": 30,
        "peer_max_count": 40,
        "received_bytes_per_sec": 575716,
        "sent_bytes_per_sec": 505720
    }
}
```

## Response Parameters Definition

| Parameter | Data Type | Description |
| :--- | :--- | :--- |
| **active peers** | *Array / Object* | The information about the active peers |
| **id** | *string* | The ID of the active peer (within an active peer object) |
| **addr** | *string* | The address of the active peer (within an active peer object) |
| **account\_id** | *string* | The identifier for the account (within an active peer object) |
| **num\_active\_peers** | *integer* | The number of active peers |
| **peer\_max\_count** | *integer* | The maximum count of the peers that can be connected |
| **sent\_bytes\_per\_sec** | *integer* | The amount of bytes sent per second |
| **received\_bytes\_per\_sec** | *integer* | The amount of bytes received per second |
| **known_producers** | *Array / Object* | The known producers |
| **account\_id** | *string* | The identifier for the account (within a known producer object) |
| **addr** | *string* | The peer address (within a known producer object) |
| **peer\_id** | *string* | The account ID of the peer (within a known producer object) |

## Use Cases

- Monitor the network connectivity and peer count of your NEAR node.
- Ensure your node is running the latest software and protocol version.
- Diagnose synchronisation or peer connection issues in validator setups.

## Code Example

**Node(Axios)**
```js
  import axios from "axios";
  let data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "network_info",
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
    "method": "network_info",
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

| **HTTP Code**                 | **Error Message**              | **Description**                                              |
| ----------------------------- | ------------------------------ | ------------------------------------------------------------ |
| **403 Forbidden**          | `RBAC: access denied`         | Missing or invalid GetBlock access token.                         |
| **500 Internal Server Error** | `Node communication error`     | Network issues            |


## Integration with Web3

By integrating `network_info` into dApp, developers can:

- Display **real-time node and peer statistics** in dApp dashboards.
- Automatically verify **network status** before performing heavy operations.
- Build **multi-node orchestration tools** to manage node fleets efficiently.
- Enhance **validator monitoring systems** with live connectivity metrics.

