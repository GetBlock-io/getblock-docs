---
description: >-
    Example code for the /v1/accounts/{account_hash}/modules JSON-RPC method. Сomplete guide on how to use /v1/accounts/{account_hash}/modules json-rpc in GetBlock.io Web3 documentation.
---

# /v1/accounts/{account\_hash}/modules - Aptos

This endpoint gets all modules(known as smart contracts)’ bytecode deployed under a specific account at a specific ledger version. These modules define smart contract logic and can include structs, resources, and functions.

---

## Supported Network
- Mainnet

## Parameters

| Parameter        | Data Type | Description                                  | Required | In    |
| ---------------- | --------- | -------------------------------------------- | -------- | ----- |
| `account_hash`   | string    | Aptos account address                        | Yes      | Path  |
| `ledger_version` | string    | The ledger version to get the account state  | No       | Query |
| `start`          | string    | The starting point for getting the resources | No       | Query |

---

## Request Example

**Base URL**

```bash
https://go.getblock.io/<ACCESS-TOKEN>/
```
**Example(cURL)**

```curl
curl --location 'https://go.getblock.io/<ACCESS_TOKEN>/v1/accounts/0x190d44266241744264b964a37b8f09863167a12d3e70cda39376cfb4e3561e12/modules?ledger_version=3362752463&limit=2'
```
---

## Response

```json
[
    {
        "bytecode": "0xa11ceb0b050000000801000203022305251d0742470889012006a9012e10d7014f0ca602c703000000010001000002020100000303010000040004000005050400000606010000070401000203030103030303030304040401040204040102000202030403030404046d617468076d696e5f753634076d756c5f6469760c6d756c5f6469765f753132380b6d756c5f746f5f753132380c6f766572666c6f775f61646406706f775f31300473717274190d44266241744264b964a37b8f09863167a12d3e70cda39376cfb4e3561e120308d0070000000000000410ffffffffffffffffffffffffffffffff0410ffffffffffffffff0000000000000000126170746f733a3a6d657461646174615f76303b01d007000000000000124552525f4449564944455f42595f5a45524f1e5768656e20747279696e6720746f20646976696465206279207a65726f2e00010000010c0a000a0123030505080b000c02050a0b010c020b02020101000004120a020600000000000000002203060700270b00350b0135180b02351a0c030b03340202010000040f0a0232000000000000000000000000000000002203060700270b000b01180b021a0c030b0334020301000007060b00350b0135180204010000042207010a01170c020a020a00230309050f0b000b02173201000000000000000000000000000000170207010a00170c020a020a01230318051e0b010b0217320100000000000000000000000000000017020b000b0116020501000008150601000000000000000c0231000c01280a010a0023030a05130b02060a00000000000000180c020b013101160c0105040b020206010000092f0a00320400000000000000000000000000000023030505120b00320000000000000000000000000000000021030a050d0600000000000000000c01050f0601000000000000000c010b010c02052d0a000c040a0032020000000000000000000000000000001a3201000000000000000000000000000000160c030a030a0423031f052a0a030c040a000a031a0b031632020000000000000000000000000000001a0c03051a0b04340c020b020200",
        "abi": {
            "address": "0x190d44266241744264b964a37b8f09863167a12d3e70cda39376cfb4e3561e12",
            "name": "math",
            "friends": [],
            "exposed_functions": [
                {
                    "name": "min_u64",
                    "visibility": "public",
                    "is_entry": false,
                    "is_view": false,
                    "generic_type_params": [],
                    "params": [
                        "u64",
                        "u64"
                    ],
                    "return": [
                        "u64"
                    ]
                },
                {
                    "name": "mul_div",
                    "visibility": "public",
                    "is_entry": false,
                    "is_view": false,
                    "generic_type_params": [],
                    "params": [
                        "u64",
                        "u64",
                        "u64"
                    ],
                    "return": [
                        "u64"
                    ]
                },
                {
                    "name": "mul_div_u128",
                    "visibility": "public",
                    "is_entry": false,
                    "is_view": false,
                    "generic_type_params": [],
                    "params": [
                        "u128",
                        "u128",
                        "u128"
                    ],
                    "return": [
                        "u64"
                    ]
                },
                {
                    "name": "mul_to_u128",
                    "visibility": "public",
                    "is_entry": false,
                    "is_view": false,
                    "generic_type_params": [],
                    "params": [
                        "u64",
                        "u64"
                    ],
                    "return": [
                        "u128"
                    ]
                },
                {
                    "name": "overflow_add",
                    "visibility": "public",
                    "is_entry": false,
                    "is_view": false,
                    "generic_type_params": [],
                    "params": [
                        "u128",
                        "u128"
                    ],
                    "return": [
                        "u128"
                    ]
                },
                {
                    "name": "pow_10",
                    "visibility": "public",
                    "is_entry": false,
                    "is_view": false,
                    "generic_type_params": [],
                    "params": [
                        "u8"
                    ],
                    "return": [
                        "u64"
                    ]
                },
                {
                    "name": "sqrt",
                    "visibility": "public",
                    "is_entry": false,
                    "is_view": false,
                    "generic_type_params": [],
                    "params": [
                        "u128"
                    ],
                    "return": [
                        "u64"
                    ]
                }
            ],
            "structs": []
        }
    },
    {
        "bytecode": "0xa11ceb0b050000000b01000402040c03101e042e0c053a0f07497e08c701400687020a109102300ac1020a0ccb023e00000101000200000003000001090700000400000100000500010100000600010100000700010100010a000301000302040204040405020201020001010109000108020108000108010663757276657309747970655f696e666f06537461626c650c556e636f7272656c61746564126173736572745f76616c69645f63757276650969735f737461626c650f69735f756e636f7272656c617465640e69735f76616c69645f63757276650b64756d6d795f6669656c640854797065496e666f07747970655f6f66190d44266241744264b964a37b8f09863167a12d3e70cda39376cfb4e3561e12000000000000000000000000000000000000000000000000000000000000000103081127000000000000126170746f733a3a6d657461646174615f76301c011127000000000000114552525f494e56414c49445f43555256450000020108010102010801000100000005380003040700270201010000000438013802210202010000000438013803210203010000010a380403030506080c00050838050c000b000200",
        "abi": {
            "address": "0x190d44266241744264b964a37b8f09863167a12d3e70cda39376cfb4e3561e12",
            "name": "curves",
            "friends": [],
            "exposed_functions": [
                {
                    "name": "assert_valid_curve",
                    "visibility": "public",
                    "is_entry": false,
                    "is_view": false,
                    "generic_type_params": [
                        {
                            "constraints": []
                        }
                    ],
                    "params": [],
                    "return": []
                },
                {
                    "name": "is_stable",
                    "visibility": "public",
                    "is_entry": false,
                    "is_view": false,
                    "generic_type_params": [
                        {
                            "constraints": []
                        }
                    ],
                    "params": [],
                    "return": [
                        "bool"
                    ]
                },
                {
                    "name": "is_uncorrelated",
                    "visibility": "public",
                    "is_entry": false,
                    "is_view": false,
                    "generic_type_params": [
                        {
                            "constraints": []
                        }
                    ],
                    "params": [],
                    "return": [
                        "bool"
                    ]
                },
                {
                    "name": "is_valid_curve",
                    "visibility": "public",
                    "is_entry": false,
                    "is_view": false,
                    "generic_type_params": [
                        {
                            "constraints": []
                        }
                    ],
                    "params": [],
                    "return": [
                        "bool"
                    ]
                }
            ],
            "structs": [
                {
                    "name": "Stable",
                    "is_native": false,
                    "is_event": false,
                    "abilities": [],
                    "generic_type_params": [],
                    "fields": [
                        {
                            "name": "dummy_field",
                            "type": "bool"
                        }
                    ]
                },
                {
                    "name": "Uncorrelated",
                    "is_native": false,
                    "is_event": false,
                    "abilities": [],
                    "generic_type_params": [],
                    "fields": [
                        {
                            "name": "dummy_field",
                            "type": "bool"
                        }
                    ]
                }
            ]
        }
    }
]
```

## Response Parameter Definition

| Field                 | Description                                                   |
| --------------------- | ------------------------------------------------------------- |
| `bytecode`            | The bytecode representation of the module                     |
| `abi`                 | The Application Binary Interface (ABI) of the Move module     |
| `address`             | The address of the Aptos account                              |
| `name`                | The name of the smart contract                                |
| `friends`             | The module friends                                            |
| `exposed_functions`   | The public functions of the module                            |
| `name (function)`     | The function name                                             |
| `visibility`          | The visibility of the function (public, private, or friend)   |
| `is_entry`            | Indicates if the function is an entry function                |
| `is_view`             | Indicates if the function is a view function                  |
| `generic_type_params` | The generic type parameters associated with the Move function |
| `constraints`         | Any constraints associated with the function                  |
| `params`              | The parameters associated with the Move function              |
| `return`              | The return type of the function                               |
| `structs`             | The list of structs defined within the module                 |
| `name (struct)`       | The struct name                                               |
| `is_native`           | Indicates if the module is native (implemented outside Move)  |
| `abilities`           | The list of abilities or capabilities provided by the module  |
| `fields`              | The list of fields defined within the struct                  |
| `type`                | The field type                                                |

---
## Use Cases

This endpoint can be used to:

- Verify what functions and structs are exposed to dApps.

- Assist in smart contract audits by fetching bytecode and ABIs.

- Identify available functions, structs, and bytecode before interacting with a contract.

---

## Code Examples
**Python (Requests)**

```python
import requests

url = "https://go.getblock.io/<ACCESS_TOKEN>/v1/accounts/0x190d44266241744264b964a37b8f09863167a12d3e70cda39376cfb4e3561e12/modules?ledger_version=3362752463&limit=2"
response = requests.request("GET", url, headers=headers, data=payload)

print(response.text)
```

**Node(Axios)**
```js
import axios from ‘axios’;

let config = {
  method: 'get',
  maxBodyLength: Infinity,
  url: 'https://go.getblock.io/<ACCESS_TOKEN>/v1/accounts/0x190d44266241744264b964a37b8f09863167a12d3e70cda39376cfb4e3561e12/modules?ledger_version=3362752463&limit=2',
};

axios.request(config)
.then((response) => {
  console.log(JSON.stringify(response.data));
})
.catch((error) => {
  console.log(error);
});
```
---

## Error handling
Some of the errors you may face are:
- `403`, which means your access token is incorrect or does not exist. To resolve this, you need to get a new access token or recheck your access token. 
- `500`, which means a Node or network issue. Retry later
- `404`, which means the resources were not found. To resolve this, use the right account_hash, or it is not a smart contract address. 

---
## Integration with Web3
By integrating /v1/accounts/{account_hash}/modules, developers can:
- Build contract explorers that Show deployed modules, their functions, and structs on user-friendly dashboards.
- Enable smart contract interactions
- Integrate module inspection for debugging, auditing, and testing workflows.
- dApps can query other on-chain modules to understand what functions are available for reuse.
