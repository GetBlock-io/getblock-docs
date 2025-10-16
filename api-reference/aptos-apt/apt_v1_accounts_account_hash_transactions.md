---
description: >-
  Example code for the /v1/accounts/{account_hash}/transactions json-rpc method.
  Сomplete guide on how to use /v1/accounts/{account_hash}/transactions json-rpc
  in GetBlock.io Web3 documentation.
---

# /v1/accounts/{account_hash}/transactions - Aptos

This endpoint gets the on-chain committed transactions associated with a specific account. This includes submitted, executed, or pending transactions where the account is the sender.


## Supported Network

- Mainnet

## Parameters

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
   <td>account_hash
   </td>
   <td>string
   </td>
   <td>Aptos account address
   </td>
   <td>Yes
   </td>
   <td>Path
   </td>
  </tr>
  <tr>
   <td>start
   </td>
   <td>string
   </td>
   <td>The starting point or offset for retrieving resources. If not provided, defaults to showing the latest transactions
   </td>
   <td>No
   </td>
   <td>query
   </td>
  </tr>
  <tr>
   <td>limit
   </td>
   <td>integer
   </td>
   <td>The maximum number of resources to retrieve per request. If not provided, defaults to default page size
   </td>
   <td>No
   </td>
   <td>query
   </td>
  </tr>
</table>



## Request


**Base URL**

```bash
https://go.getblock.io/<ACCESS_TOKEN>
```

**Example(cURl)**

```
curl -X GET "https://go.getblock.io/<ACCESS_TOKEN>/v1/accounts/0xbf9239be9eb7e7a3d8e4c1f36083464fd47e6bd1f82a43b7c0f7ee958705a52f
/transactions?limit=5"
```

## Response
```json
[
    {
        "version": "3574927316",
        "hash": "0x01ef3543f79823b3a9fbc8acac9fba4bb25c7c1abb65f533caff2d9bcc0678f6",
        "state_change_hash": "0x5cfb448ecb226172f3f5279d02658a645afdacf88a1ce308bf5b0717eca0c99e",
        "event_root_hash": "0x63937d2f2e996f5c9e6cd669e518fdf7e474af4902a11501ec15dbb8d53e5ce9",
        "state_checkpoint_hash": null,
        "gas_used": "82",
        "success": true,
        "vm_status": "Executed successfully",
        "accumulator_root_hash": "0x3d25cf24e802a777497da7f76fdeb05a472b30586f2171b82fbb72e1e3504609",
        "changes": [
            {
                "address": "0xa",
                "state_key_hash": "0x1db5441d8fa4229c5844f73fd66da4ad8176cb8793d8b3a7f6ca858722030043",
                "data": {
                    "type": "0x1::coin::PairedCoinType",
                    "data": {
                        "type": {
                            "account_address": "0x1",
                            "module_name": "0x6170746f735f636f696e",
                            "struct_name": "0x4170746f73436f696e"
                        }
                    }
                },
                "type": "write_resource"
            }

        ],
        "sender": "0xbf9239be9eb7e7a3d8e4c1f36083464fd47e6bd1f82a43b7c0f7ee958705a52f",
        "sequence_number": "871700",
        "max_gas_amount": "200000",
        "gas_unit_price": "100",
        "expiration_timestamp_secs": "1760643271",
        "payload": {
            "function": "0x487e905f899ccb6d46fdaec56ba1e0c4cf119862a16c409904b8c78fab1f5e8a::router::swap",
            "type_arguments": [],
            "arguments": [
                "0x82e0b52f95ae57b35220726a32c3415919389aa5b8baa33a058d7125797535cc01000000000000000000000000000000683e030100000000000000000000000000000000000000000000000000000000d093f60000000000000000000000000000000000000000000000000000000000"
            ],
            "type": "entry_function_payload"
        },
        "signature": {
            "public_key": "0x7df17b23676ef29e040847e64ae2a8351819d4bfaf64f3bfe2124d92156c1c02",
            "signature": "0x8001b27ddb488c1ad5f4f6ed7fe4886b42528b0646a05685469b524b67cc6298192fa42d9af5ff2df1791c88f2f16dfdc22c8c1d5060795c68161a25ef335d01",
            "type": "ed25519_signature"
        },
        "replay_protection_nonce": null,
        "events": [
            {
                "guid": {
                    "creation_number": "0",
                    "account_address": "0x0"
                },
                "sequence_number": "0",
                "type": "0xa611a8ba7261ed1f4d3afe4ac2166fc9f3180103e3296772d593a1e2720c7405::stable::IncentiveSynced",
                "data": {
                    "accumulated_rewards_per_share": "33902212381193",
                    "campaign_idx": "15",
                    "last_accumulation_time": "1760643255",
                    "last_total_shares": "4992836695498717952869616",
                    "pool_addr": "0x82e0b52f95ae57b35220726a32c3415919389aa5b8baa33a058d7125797535cc",
                    "total_distributed": "25151305402",
                    "ts": "1760643255618178"
                }
            },
            {
                "guid": {
                    "creation_number": "0",
                    "account_address": "0x0"
                },
                "sequence_number": "0",
                "type": "0xa611a8ba7261ed1f4d3afe4ac2166fc9f3180103e3296772d593a1e2720c7405::stable::Swapped",
                "data": {
                    "bought_id": "0",
                    "buyer": "0xbf9239be9eb7e7a3d8e4c1f36083464fd47e6bd1f82a43b7c0f7ee958705a52f",
                    "pool_addr": "0x82e0b52f95ae57b35220726a32c3415919389aa5b8baa33a058d7125797535cc",
                    "sold_id": "1",
                    "stored_balances": [
                        "1990058935750",
                        "3042379974793"
                    ],
                    "tokens_bought": "16980317",
                    "tokens_sold": "16989240",
                    "ts": "1760643255618178"
                }
            },
        "timestamp": "1760643255618178",
        "type": "user_transaction"
    }
]
```

### Response Parameter Definition

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
   <td>The transaction version (ledger sequence number).
   </td>
  </tr>
  <tr>
   <td>hash
   </td>
   <td>String
   </td>
   <td>Unique hash of the transaction.
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
   <td>Merkle root of events emitted in this transaction.
   </td>
  </tr>
  <tr>
   <td>gas_used
   </td>
   <td>String
   </td>
   <td>Amount of gas consumed by the transaction.
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
   <td>Execution status from the Aptos VM.
   </td>
  </tr>
  <tr>
   <td>accumulator_root_hash
   </td>
   <td>String
   </td>
   <td>Root hash of the transaction accumulator after applying this txn.
   </td>
  </tr>
  <tr>
   <td>changes
   </td>
   <td>Array
   </td>
   <td>List of state changes caused by the transaction.
   </td>
  </tr>
  <tr>
   <td>changes.address
   </td>
   <td>String
   </td>
   <td>Address whose resource was modified.
   </td>
  </tr>
  <tr>
   <td>changes.state_key_hash
   </td>
   <td>String
   </td>
   <td>State key hash for the modified resource.
   </td>
  </tr>
  <tr>
   <td>changes.type
   </td>
   <td>String
   </td>
   <td>Type of state change (e.g., write_resource).
   </td>
  </tr>
  <tr>
   <td>changes.data
   </td>
   <td>Object
   </td>
   <td>Modified resource data.
   </td>
  </tr>
  <tr>
   <td>changes.data.type
   </td>
   <td>String
   </td>
   <td>Type of resource (e.g., CoinStore&lt;AptosCoin>).
   </td>
  </tr>
  <tr>
   <td>changes.data.data
   </td>
   <td>Object
   </td>
   <td>Full contents of the updated resource (balance, events, frozen state, etc).
   </td>
  </tr>
</table>


## Use Cases

This method can be used for:
* Get an account’s transaction history.
* Build **wallet activity feeds** (incoming/outgoing transfers).
* Build **block explorers** that show user-specific transactions.
* Track failed vs successful transactions.


## Code Example

**Python Request**

```python
import requests

url = "https://go.getblock.io/<ACCESS_TOKEN>/v1/accounts/0xc20ea5a196c81d8d7aff814aa37f8a5823acffbc4193efd3b2aafc9ef2803255/transactions"

response = requests.request("GET", url, headers=headers, data=payload)

print(response.text)


```

**Node(axios)**
```js
import axios from 'axios';

let config = {
  method: 'get',
  maxBodyLength: Infinity,
  url: 'https://go.getblock.io/<ACCESS_TOKEN>/v1/accounts/0xc20ea5a196c81d8d7aff814aa37f8a5823acffbc4193efd3b2aafc9ef2803255/transactions',
  headers: { }
};

axios.request(config)
.then((response) => {
  console.log(JSON.stringify(response.data));
})
.catch((error) => {
  console.log(error);
});

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
   <td>403
   </td>
   <td>Forbidden
   </td>
   <td>Missing or invalid ACCESS_TOKEN.
   </td>
  </tr>
  <tr>
   <td>404
   </td>
   <td>Resource not found
   </td>
   <td>The given resource type does not exist for this account.
   </td>
  </tr>
  <tr>
   <td>500
   </td>
   <td>Internal server error
   </td>
   <td>Node or network issue. Retry later.
   </td>
  </tr>
</table>

## Integration with Web3

By integrating /v1/accounts/{account_hash}/transactions into developers can:

* **Build transaction history dashboards to** show a user’s past on-chain actions. 
* Track when a user’s transaction is confirmed or fails.
* dApps can fetch all transactions from an account for financial or legal reporting.
* **a**nalyse user behaviour, token transfers, and contract interactions for dApps.
