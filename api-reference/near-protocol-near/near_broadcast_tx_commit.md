---
description: >-
  Example code for the broadcast_tx_commit json-rpc method. Сomplete guide on
  how to use broadcast_tx_commit json-rpc in GetBlock.io Web3 documentation.
---

# broadcast\_tx\_commit - NEAR Protocol

This method is used to send a signed transaction to the NEAR blockchain and wait until it is fully confirmed on-chain. Unlike `/broadcast_tx_async`, this endpoint waits for finalisation, returning the **execution result** of the transaction (success or failure).


## Supported Network

- Mainnet

## Parameters Description

| **Parameter**        | **Type** | **Required** | **Description**                                   |
| -------------------- | -------- | ------------ | ------------------------------------------------- |
| `signed_tx_base64` | string   |  Yes        | The signed transaction, encoded in Base64 format. |

## Request Example

**Base URL**
  ```bash
     https://go.getblock.io/<ACCESS_TOKEN>
  ```

**Example(cURL)**

  ```bash
  curl -X POST https://go.getblock.io/<ACCESS_TOKEN>\
  -H "Content-Type: application/json" \
  -d '{"jsonrpc": "2.0",
"method": "broadcast_tx_commit",
"params": ["DgAAAHNlbmRlci50ZXN0bmV0AOrmAai64SZOv9e/naX4W15pJx0GAap35wTT1T/DwcbbDQAAAAAAAAAQAAAAcmVjZWl2ZXIudGVzdG5ldIODI4YfV/QS++blXpQYT+bOsRblTRW4f547y/LkvMQ9AQAAAAMAAACh7czOG8LTAAAAAAAAAAXcaTJzu9GviPT7AD4mNJGY79jxTrjFLoyPBiLGHgBi8JK1AnhK8QknJ1ourxlvOYJA2xEZE8UR24THmSJcLQw="],
"id": "getblock.io"}'
  ```

## Response

```json
  {
      "jsonrpc": "2.0",
      "id": "getblock.io",
      "result": {
          "status": {
              "SuccessValue": ""
          },
          "transaction": {
              "signer_id": "sender.testnet",
              "public_key": "ed25519:Gowpa4kXNyTMRKgt5W7147pmcc2PxiFic8UHW9rsNvJ6",
              "nonce": 13,
              "receiver_id": "receiver.testnet",
              "actions": [
                  {
                      "Transfer": {
                          "deposit": "1000000000000000000000000"
                      }
                  }
              ],
              "signature": "ed25519:7oCBMfSHrZkT7tzPDBxxCd3tWFhTES38eks3MCZMpYPJRfPWKxJsvmwQiVBBxRLoxPTnXVaMU2jPV3MdFKZTobH",
              "hash": "ASS7oYwGiem9HaNwJe6vS2kznx2CxueKDvU9BAYJRjNR"
          },
          "transaction_outcome": {
              "proof": [],
              "block_hash": "9MzuZrRPW1BGpFnZJUJg6SzCrixPpJDfjsNeUobRXsLe",
              "id": "ASS7oYwGiem9HaNwJe6vS2kznx2CxueKDvU9BAYJRjNR",
              "outcome": {
                  "logs": [],
                  "receipt_ids": [
                      "BLV2q6p8DX7pVgXRtGtBkyUNrnqkNyU7iSksXG7BjVZh"
                  ],
                  "gas_burnt": 223182562500,
                  "tokens_burnt": "22318256250000000000",
                  "executor_id": "sender.testnet",
                  "status": {
                      "SuccessReceiptId": "BLV2q6p8DX7pVgXRtGtBkyUNrnqkNyU7iSksXG7BjVZh"
                  }
              }
          },
          "receipts_outcome": [
              {
                  "proof": [],
                  "block_hash": "5Hpj1PeCi32ZkNXgiD1DrW4wvW4Xtic74DJKfyJ9XL3a",
                  "id": "BLV2q6p8DX7pVgXRtGtBkyUNrnqkNyU7iSksXG7BjVZh",
                  "outcome": {
                      "logs": [],
                      "receipt_ids": [
                          "3sawynPNP8UkeCviGqJGwiwEacfPyxDKRxsEWPpaUqtR"
                      ],
                      "gas_burnt": 223182562500,
                      "tokens_burnt": "22318256250000000000",
                      "executor_id": "receiver.testnet",
                      "status": {
                          "SuccessValue": ""
                      }
                  }
              },
              {
                  "proof": [],
                  "block_hash": "CbwEqMpPcu6KwqVpBM3Ry83k6M4H1FrJjES9kBXThcRd",
                  "id": "3sawynPNP8UkeCviGqJGwiwEacfPyxDKRxsEWPpaUqtR",
                  "outcome": {
                      "logs": [],
                      "receipt_ids": [],
                      "gas_burnt": 0,
                      "tokens_burnt": "0",
                      "executor_id": "sender.testnet",
                      "status": {
                          "SuccessValue": ""
                      }
                  }
              }
          ]
      }
  }
  ```

## Response Parameters Definition

| **Field**                                        | **Type** | **Description**                                                                |
| ------------------------------------------------ | -------- | ------------------------------------------------------------------------------ |
| **status**                                       | object   | Overall status of the transaction execution.                                   |
| **status.successValue**                          | string   | Amount successfully transferred or result of the execution.                    |
| **transaction**                                  | object   | The transaction object.                                                        |
| **transaction.signer_id**                        | string   | Account ID of the transaction originator.                                      |
| **transaction.public_key**                       | string   | Public key of the signer.                                                      |
| **transaction.nonce**                            | string   | Number of transactions previously sent by the sender (hexadecimal).            |
| **transaction.receiver_id**                      | string   | Account ID of the transaction receiver.                                        |
| **transaction.actions**                          | array    | List of actions included in the transaction (from `near-api-js.transactions`). |
| **transaction.actions.transfer**                 | object   | Transfer action details.                                                       |
| **transaction.actions.transfer.deposit**         | string   | Amount of tokens being transferred.                                            |
| **transaction.signature**                        | string   | Standard `ed25519` signature of the transaction.                               |
| **transaction.hash**                             | string   | Hash of the transaction.                                                       |
| **transaction_outcome**                          | object   | Result of the transaction execution.                                           |
| **transaction_outcome.proof**                    | object   | Proof of inclusion of the transaction in a block.                              |
| **transaction_outcome.block_hash**               | string   | Hash of the block that included this transaction.                              |
| **transaction_outcome.id**                       | string   | Transaction ID.                                                                |
| **transaction_outcome.outcome**                  | object   | Outcome details (execution result, logs, receipts).                            |
| **transaction_outcome.outcome.logs**             | array    | Logs generated during transaction execution.                                   |
| **transaction_outcome.outcome.receipt_ids**      | array    | IDs of the receipts created by the transaction.                                |
| **transaction_outcome.outcome.gas_burnt**        | string   | Gas units consumed during execution.                                           |
| **transaction_outcome.outcome.tokens_burnt**     | string   | Amount of NEAR tokens spent as gas.                                            |
| **transaction_outcome.outcome.executor_id**      | string   | ID of the account executing the transaction.                                   |
| **transaction_outcome.outcome.status**           | object   | Status of the transaction execution (`SuccessValue`, failure, etc.).           |
| **transaction_outcome.outcome.successReceiptId** | string   | Transaction ID of the successfully executed receipt.                           |
| **transaction_outcome.receipts_outcome**         | array    | Array of outcomes for all receipts generated by the transaction.               |


## Use Cases

* Execute smart contract functions (e.g., token transfers, staking operations).
* Wait for transaction confirmation before updating app state.
* Build transaction verification systems or block explorers.
* Test contract calls in development networks (testnet).

## Code Example

**Node(Axios)**
  ```js
  import axios from "axios";
  let data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "broadcast_tx_commit",
    "params": [
      "DgAAAHNlbmRlci50ZXN0bmV0AOrmAai64SZOv9e/naX4W15pJx0GAap35wTT1T/DwcbbDQAAAAAAAAAQAAAAcmVjZWl2ZXIudGVzdG5ldIODI4YfV/QS++blXpQYT+bOsRblTRW4f547y/LkvMQ9AQAAAAMAAACh7czOG8LTAAAAAAAAAAXcaTJzu9GviPT7AD4mNJGY79jxTrjFLoyPBiLGHgBi8JK1AnhK8QknJ1ourxlvOYJA2xEZE8UR24THmSJcLQw="
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

**Python(Request)**
  ```python
  import requests
  import json
  
  url = "https://go.getblock.io/<ACCESS_TOKEN>"
  
  payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "broadcast_tx_commit",
    "params": [
      "DgAAAHNlbmRlci50ZXN0bmV0AOrmAai64SZOv9e/naX4W15pJx0GAap35wTT1T/DwcbbDQAAAAAAAAAQAAAAcmVjZWl2ZXIudGVzdG5ldIODI4YfV/QS++blXpQYT+bOsRblTRW4f547y/LkvMQ9AQAAAAMAAACh7czOG8LTAAAAAAAAAAXcaTJzu9GviPT7AD4mNJGY79jxTrjFLoyPBiLGHgBi8JK1AnhK8QknJ1ourxlvOYJA2xEZE8UR24THmSJcLQw="
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

| **HTTP Code**                 | **Error Message**                   | **Description**                                                    |
| ----------------------------- | ----------------------------------- | ------------------------------------------------------------------ |
| **400 Bad Request**           | `REQUEST_VALIDATION_ERROR`        | The provided Base64 transaction data is malformed or incomplete.   |
| **403 Forbidden**          | `RBAC: access denied`   | The GetBlock API key is missing or invalid.                        |
| **404 Not Found**             | `REQUEST_VALIDATION_ERROR`             | The transaction expired.          |
| **500 Internal Server Error** | `Node error`                        | The node failed to process or broadcast the transaction.           |

## Integration with Web3

Integrating `/broadcast_tx_commit` into dApp, allows developers to:
- Automatically verify transaction outcomes in Web3 applications.
- Provide instant feedback to users after blockchain interactions.
- Synchronise backend systems with confirmed on-chain events.

