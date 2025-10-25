---
description: >-
  Example code for the tx json-rpc method. Сomplete guide on how to use tx
  json-rpc in GetBlock.io Web3 documentation.
---

# tx - NEAR Protocol

This method returns the **full details of a transaction** on the NEAR blockchain, including its status, outcome, receipts, and metadata.


## Supported Networks

- Mainnet

## Parameters Description

| **Parameter**        | **Type** | **Required** | **Description**                                               |
| -------------------- | -------- | ------------ | ------------------------------------------------------------- |
| **transaction_hash** | string   | Yes        | The hash of the transaction to retrieve.                      |
| **signer_id**        | string   | Yes        | The NEAR account ID of the sender who signed the transaction. |

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
"method": "tx",
"params": ["Ce4DPVFyDRdx54iLoSiKA6gqwGnE5V4mTZyVvFDQsvmN","relay.aurora"],
"id": "getblock.io"}'
```

## Response Example

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
            "nonce": 15,
            "receiver_id": "receiver.testnet",
            "actions": [
                {
                    "Transfer": {
                        "deposit": "1000000000000000000000000"
                    }
                }
            ],
            "signature": "ed25519:3168QMdTpcwHvM1dmMYBc8hg9J3Wn8n7MWBSE9WrEpns6P5CaY87RM6k4uzyBkQuML38CZhU18HzmQEevPG1zCvk",
            "hash": "6zgh2u9DqHHiXzdy9ouTP7oGky2T4nugqzqt9wJZwNFm"
        },
        "transaction_outcome": {
            "proof": [
                {
                    "hash": "F7mL76CMdfbdZ3xCehVGNh1fCyaR37gr3MeGX3EZkiVf",
                    "direction": "Right"
                }
            ],
            "block_hash": "ADTMLVtkhsvzUxuf6m87Pt1dnF5vi1yY7ftxmNpFx7y",
            "id": "6zgh2u9DqHHiXzdy9ouTP7oGky2T4nugqzqt9wJZwNFm",
            "outcome": {
                "logs": [],
                "receipt_ids": [
                    "3dMfwczW5GQqXbD9GMTnmf8jy5uACxG6FC5dWxm3KcXT"
                ],
                "gas_burnt": 223182562500,
                "tokens_burnt": "22318256250000000000",
                "executor_id": "sender.testnet",
                "status": {
                    "SuccessReceiptId": "3dMfwczW5GQqXbD9GMTnmf8jy5uACxG6FC5dWxm3KcXT"
                }
            }
        },
        "receipts_outcome": [
            {
                "proof": [
                    {
                        "hash": "6h95oEd7ih62KXfyPT4zsZYont4qy9sWEXc5VQVDhqtG",
                        "direction": "Right"
                    },
                    {
                        "hash": "6DnibgZk1T669ZprcehUy1GpCSPw1kjzXRGu69nSaUNn",
                        "direction": "Right"
                    }
                ],
                "block_hash": "GgFTVr33r4MrpAiHc9mr8TZqLnpZAX1BaZTNvhBnciy2",
                "id": "3dMfwczW5GQqXbD9GMTnmf8jy5uACxG6FC5dWxm3KcXT",
                "outcome": {
                    "logs": [],
                    "receipt_ids": [
                        "46KYgN8ddxs4Qy8C7BDQH49XUfcYZsaQmAvdU1nfcL9V"
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
                "proof": [
                    {
                        "hash": "CD9Y7Bw3MSFgaPZzpc1yP51ajhGDCAsR61qXcMNcRoHf",
                        "direction": "Left"
                    }
                ],
                "block_hash": "EGAgKuW6Bd6QKYSaxAkx2pPGmnjrjAcq4UpoUiqMXvPH",
                "id": "46KYgN8ddxs4Qy8C7BDQH49XUfcYZsaQmAvdU1nfcL9V",
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

| **Field**                                              | **Type** | **Description**                                                |
| ------------------------------------------------------ | -------- | -------------------------------------------------------------- |
| **receipts_outcome**                                   | array    | The receipts outcome for the transaction.                      |
| **receipts_outcome.block_hash**                        | string   | The hash of the block this transaction was associated with.    |
| **receipts_outcome.id**                                | string   | The transaction ID.                                            |
| **receipts_outcome.outcome**                           | object   | The outcome associated with the transaction.                   |
| **receipts_outcome.outcome.executor_id**               | string   | The ID of the transaction originator.                          |
| **receipts_outcome.outcome.gas_burnt**                 | integer  | The gas burnt while executing the transaction.                 |
| **receipts_outcome.outcome.logs**                      | array    | Logs for the application being executed by this transaction.   |
| **receipts_outcome.outcome.metadata**                  | object   | Describes transaction metadata and contextual information.     |
| **receipts_outcome.outcome.gas_profile**               | array    | Details about gas usage, cost, and category.                   |
| **receipts_outcome.outcome.gas_profile.cost**          | string   | The cost of the gas used.                                      |
| **receipts_outcome.outcome.gas_profile.cost_category** | string   | The category of the gas cost (e.g., compute, storage).         |
| **receipts_outcome.outcome.gas_profile.gas_used**      | string   | The total amount of gas used.                                  |
| **receipts_outcome.outcome.version**                   | string   | Version number of the transaction execution.                   |
| **receipts_outcome.outcome.receipt_ids**               | array    | The IDs of receipts generated by the transaction.              |
| **receipts_outcome.outcome.status**                    | string   | The status of the transaction (e.g., Success, Failure).        |
| **receipts_outcome.outcome.successReceiptId**          | string   | The transaction ID of the successful receipt.                  |
| **receipts_outcome.outcome.tokens_burnt**              | string   | The amount of tokens burnt during execution.                   |
| **receipts_outcome.proof**                             | array    | The cryptographic proof for the transaction.                   |
| **receipts_outcome.proof.hash**                        | string   | The hash of the proof.                                         |
| **receipts_outcome.proof.direction**                   | string   | The proof direction.                                           |
| **receipts_outcome.status**                            | string   | Final transaction status.                                      |
| **receipts_outcome.successValue**                      | string   | Value successfully sent after execution.                       |
| **transaction**                                        | object   | The transaction object containing metadata and action details. |
| **transaction.actions**                                | array    | Actions performed within the transaction.                      |
| **transaction.actions.FunctionCall**                   | object   | Represents a function call action on the `receiver_id`.        |
| **transaction.actions.FunctionCall.args**              | string   | Transaction-specific arguments (Base64-encoded).               |
| **transaction.actions.FunctionCall.deposit**           | string   | Amount of tokens deposited (in yoctoNEAR).                     |
| **transaction.actions.FunctionCall.gas**               | integer  | Gas allocated for function execution.                          |
| **transaction.actions.FunctionCall.method_name**       | string   | The method being called on the contract.                       |
| **transaction.hash**                                   | string   | Hash of the transaction.                                       |
| **transaction.nonce**                                  | integer  | Number of prior transactions from the signer (nonce).          |
| **transaction.public_key**                             | string   | The public key of the signer.                                  |
| **transaction.receiver_id**                            | string   | The account ID of the transaction receiver.                    |
| **transaction.signature**                              | string   | The standard `ed25519` signature type.                         |
| **transaction.signer_id**                              | string   | The account ID of the transaction originator.                  |
| **transaction_outcome**                                | object   | The main transaction outcome object.                           |
| **transaction_outcome.block_hash**                     | string   | The hash of the block this transaction was associated with.    |
| **transaction_outcome.id**                             | string   | The transaction ID.                                            |
| **transaction_outcome.outcome**                        | object   | The outcome associated with transaction execution.             |
| **transaction_outcome.outcome.executor_id**            | string   | The executor of the transaction.                               |
| **transaction_outcome.outcome.gas_burnt**              | integer  | Gas consumed during execution.                                 |
| **transaction_outcome.outcome.logs**                   | array    | Logs generated by the transaction.                             |
| **transaction_outcome.outcome.metadata**               | object   | Describes transaction execution metadata.                      |
| **transaction_outcome.outcome.gas_profile**            | array    | Details of gas consumption and categorization.                 |
| **transaction_outcome.outcome.version**                | string   | Version number of transaction outcome.                         |
| **transaction_outcome.outcome.receipt_ids**            | array    | The IDs of the receipts generated.                             |
| **transaction_outcome.outcome.status**                 | string   | Status of the transaction execution.                           |
| **transaction_outcome.outcome.successReceiptId**       | string   | ID of the successful receipt.                                  |
| **transaction_outcome.outcome.tokens_burnt**           | string   | Total tokens burnt.                                            |
| **transaction_outcome.proof**                          | array    | Cryptographic proof of the transaction.                        |
| **transaction_outcome.proof.hash**                     | string   | Hash value of the proof.                                       |
| **transaction_outcome.proof.direction**                | string   | Direction of the proof in Merkle path.                         |


## Use Cases

* Retrieve **transaction status** and validate results.
* Track **gas consumption and receipts**.
* Display **user transaction history** in wallets or explorers.
* Debug and **trace smart contract executions**.


## Code Example

**Node(axios)**

```js
import axios from 'axios';
let data = JSON.stringify({
  "jsonrpc": "2.0",
  "method": "tx",
  "params": [
    "Ce4DPVFyDRdx54iLoSiKA6gqwGnE5V4mTZyVvFDQsvmN",
    "relay.aurora"
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
  "method": "tx",
  "params": [
    "Ce4DPVFyDRdx54iLoSiKA6gqwGnE5V4mTZyVvFDQsvmN",
    "relay.aurora"
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

| **HTTP Code**                 | **Error Message**                       | **Description**                                             |
| ----------------------------- | --------------------------------------- | ----------------------------------------------------------- |
| **400 Bad Request**           | `Invalid transaction hash or signer_id` | Missing or malformed transaction parameters.                |
| **403 Forbidden**             | `RBAC: access denied`                 | The getblock access token is missing or incrrect |
| **500 Internal Server Error** | `Node processing error`                 | The NEAR node failed to process the request.                |
| **503 Service Unavailable**   | `Node temporarily unavailable`          | The node is syncing or temporarily offline.                 |

## Integration with Web3

By integrating `tx` into dApp, developers can:

- Build **blockchain explorers** and **wallet transaction trackers**.
- Verify transaction confirmations and receipts.
- Automate **transaction monitoring** for smart contracts and bots.
- Improve **user experience** by showing accurate success/failure messages.
