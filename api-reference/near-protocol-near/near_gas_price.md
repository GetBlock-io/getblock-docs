---
description: >-
  Example code for the gas_price json-rpc method. Сomplete guide on how to use
  gas_price json-rpc in GetBlock.io Web3 documentation.
---

# gas\_price - NEAR Protocol

The method returns the **gas price** for a specific block height or the **latest block** if no height is provided.

## Supported Networks

- Mainnet

## Parameters Description

| Parameter| Type | Description |
| :--- | :--- | :--- |
| `block_height` | `string` | The height of the block |
| `block_hash` | `string` | The hash of the block |
| `null` | N/A | Using `null` will return the most recent block's gas price |

## Request Example

**Base URL**
  ```bash
  https://go.getblock.io/<ACCESS_TOKEN>
  ```
**Example(cURL)**

  ```curl
  curl -X POST https://go.getblock.io/<ACCESS_TOKEN>\
  -H "Content-Type: application/json" \
  -d '{"jsonrpc": "2.0",
  "method": "gas_price",
  "params": [169528908],
  "id": "getblock.io"}'
  ```

## Response Example

  ```json
  {
      "jsonrpc": "2.0",
      "result": {
          "gas_price": "100000000"
      },
      "id": "getblock.io"
  }
  ```

## Response Parameters Definition

| **Field**     | **Type** | **Description**                                                                                |
| ------------- | -------- | ---------------------------------------------------------------------------------------------- |
| **gas_price** | string   | The gas price used for the specified block, expressed in yoctoNEAR (1 yoctoNEAR = 10⁻²⁴ NEAR). |


## Use Cases

* Estimate the **cost of deploying or executing** smart contracts.
* Dynamically calculate **transaction fees** based on current network conditions.
* Integrate gas price monitoring into dApps or wallets for cost optimisation.
* Predict gas expenditure during **high network congestion** periods.

## Code Example

**Node(Axios)**
```js
   import axios from "axios";
  let data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "gas_price",
    "params": [
      169528908
    ],
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
    "method": "gas_price",
    "params": [
      169528908
    ],
    "id": "getblock.io"
  })
  headers = {
    'Content-Type': 'application/json'
  }
  response = requests.request("POST", url, headers=headers, data=payload)
  print(response.text)
```


## Error Handling

| **HTTP Code**                 | **Error Message**                        | **Description**                                                       |
| ----------------------------- | ---------------------------------------- | --------------------------------------------------------------------- |
| **400 Bad Request**           | `UNKNOWN_BLOCK`  or `METHOD_NOT_FOUND`                  | The provided block height or hash is not valid or  Method not found                   |
| **403 Forbidden**             | `RBAC: access denied`                        | The Getblock access token is missing or not correct                   |
| **500 Internal Server Error** | `Node processing error`                  | Network Issue        |


## Integration with Web3

By integrating `gas_price` into dApp, developers can:

- Improve UX by showing users **live fee estimates**.
- Optimise smart contract executions to avoid failed transactions due to low gas prices.
- Build dashboards or analytics tools that track gas price trends over time.
