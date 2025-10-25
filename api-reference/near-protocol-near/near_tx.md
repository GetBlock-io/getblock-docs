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

| **Field**                                    | **Type** | **Description**                                                    |
| -------------------------------------------- | -------- | ------------------------------------------------------------------ |
| **status**                                   | object   | The status of the transaction (e.g., `SuccessValue` or `Failure`). |
| **transaction**                              | object   | The transaction object with metadata.                              |
| **transaction.signer_id**                    | string   | Account ID of the transaction sender.                              |
| **transaction.public_key**                   | string   | Public key of the sender.                                          |
| **transaction.nonce**                        | integer  | Nonce used for the transaction.                                    |
| **transaction.receiver_id**                  | string   | Account ID of the transaction recipient.                           |
| **transaction.actions**                      | array    | List of actions included in the transaction.                       |
| **transaction.actions.Transfer.deposit**     | string   | Amount of NEAR (in yoctoNEAR) transferred.                         |
| **transaction.signature**                    | string   | Standard `ed25519` signature of the transaction.                   |
| **transaction.hash**                         | string   | Hash identifying the transaction.                                  |
| **transaction_outcome**                      | object   | Result of the transaction execution.                               |
| **transaction_outcome.block_hash**           | string   | Block hash in which the transaction was included.                  |
| **transaction_outcome.id**                   | string   | Transaction ID.                                                    |
| **transaction_outcome.outcome.executor_id**  | string   | Account that executed the transaction.                             |
| **transaction_outcome.outcome.gas_burnt**    | integer  | Gas units burned during execution.                                 |
| **transaction_outcome.outcome.tokens_burnt** | string   | Amount of tokens burned (in yoctoNEAR).                            |
| **transaction_outcome.outcome.logs**         | array    | Logs generated by the transaction.                                 |
| **transaction_outcome.outcome.receipt_ids**  | array    | IDs of receipts generated by the transaction.                      |
| **transaction_outcome.outcome.status**       | object   | Status object showing success or failure type.                     |
| **receipts_outcome**                         | array    | List of outcomes for all generated receipts.                       |


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
