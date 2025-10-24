---
description: >-
  Example code for the broadcast_tx_async json-rpc method. Сomplete guide on how
  to use broadcast_tx_async json-rpc in GetBlock.io Web3 documentation.
---

# broadcast\_tx\_async - NEAR Protocol

This method is used to send a signed transaction to the NEAR blockchain without waiting for it to be confirmed on-chain. It only submits the transaction and immediately returns the transaction hash.


## Supported Networks

- Mainnet


## Parameters

| Parameter        | Type | Required | Description                           |
| -------------------- | -------- | ------------ | ------------------------------------------ |
| `signed_tx_base64` | string   | Yes        | The signed transaction, encoded in Base64. |



## Request Example

**Base URL**

  ```bash
      https://go.getblock.io/<ACCESS_TOKEN>
  ```
    
**Example(cURL)**
  ```bash
    curl -X POST https://go.getblock.io/<ACCESS_TOKEN> \
    -H "Content-Type: application/json" \
    -d '{"jsonrpc": "2.0",
    "method": "broadcast_tx_async",
    "params": ["DgAAAHNlbmRlci50ZXN0bmV0AOrmAai64SZOv9e/naX4W15pJx0GAap35wTT1T/DwcbbDwAAAAAAAAAQAAAAcmVjZWl2ZXIudGVzdG5ldNMnL7URB1cxPOu3G8jTqlEwlcasagIbKlAJlF5ywVFLAQAAAAMAAACh7czOG8LTAAAAAAAAAGQcOG03xVSFQFjoagOb4NBBqWhERnnz45LY4+52JgZhm1iQKz7qAdPByrGFDQhQ2Mfga8RlbysuQ8D8LlA6bQE="],
    "id": "getblock.io"}'
  ```

## Response

  ```json
  {
      "jsonrpc": "2.0",
      "result": "6zgh2u9DqHHiXzdy9ouTP7oGky2T4nugqzqt9wJZwNFm",
      "id": "getblock.io"
  }
  ```


## Response Parameter Definition

| Field  | Type | Description                                           |
| ---------- | -------- | ----------------------------------------------------------- |
| `result` | string   | The transaction hash returned immediately after submission. |
| `id`     | string   | The user-defined identifier for tracking the request.       |


## Use Cases
- Submit transactions asynchronously
- Enable batch transaction submissions for high-frequency operations.
- Improve user experience in apps that handle multiple transaction requests.

## Code Example

**Node(Axios)**
```js
     import axios from 'axios'
    let data = JSON.stringify({
      "jsonrpc": "2.0",
      "method": "broadcast_tx_async",
      "params": [
        "DgAAAHNlbmRlci50ZXN0bmV0AOrmAai64SZOv9e/naX4W15pJx0GAap35wTT1T/DwcbbDwAAAAAAAAAQAAAAcmVjZWl2ZXIudGVzdG5ldNMnL7URB1cxPOu3G8jTqlEwlcasagIbKlAJlF5ywVFLAQAAAAMAAACh7czOG8LTAAAAAAAAAGQcOG03xVSFQFjoagOb4NBBqWhERnnz45LY4+52JgZhm1iQKz7qAdPByrGFDQhQ2Mfga8RlbysuQ8D8LlA6bQE="
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
      "method": "broadcast_tx_async",
      "params": [
        "DgAAAHNlbmRlci50ZXN0bmV0AOrmAai64SZOv9e/naX4W15pJx0GAap35wTT1T/DwcbbDwAAAAAAAAAQAAAAcmVjZWl2ZXIudGVzdG5ldNMnL7URB1cxPOu3G8jTqlEwlcasagIbKlAJlF5ywVFLAQAAAAMAAACh7czOG8LTAAAAAAAAAGQcOG03xVSFQFjoagOb4NBBqWhERnnz45LY4+52JgZhm1iQKz7qAdPByrGFDQhQ2Mfga8RlbysuQ8D8LlA6bQE="
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

| HTTP Code                 | Error Message                   | Description                                            |
| ----------------------------- | ----------------------------------- | ---------------------------------------------------------- |
| **400 Bad Request**           | `REQUEST_VALIDATION_ERROR`        | The Base64-encoded transaction is malformed or incomplete. |
| **403 Forbidden**          | `RBAC: access denied`   | The GetBlock access token is incorrect or missing.         |
| **500 Internal Server Error** | `Node error`                        | The node failed to broadcast the transaction.              |

## Integration with Web3

By integrating `/broadcast_tx_async` into your dApp, developers can:

- Streamline transaction broadcasting for dApps and wallets.
- Offload transaction confirmation logic to background workers.
- Maintain smooth performance in decentralised applications by avoiding delays.
