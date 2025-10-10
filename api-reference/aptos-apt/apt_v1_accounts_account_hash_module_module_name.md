---
description: >-
    Example code for the /v1/accounts/{account_hash}/module/{module_name} JSON-RPC method. Сomplete guide on how to use /v1/accounts/{account_hash}/module/{module_name}json-rpc in GetBlock.io Web3 documentation.
---

# /v1/accounts/{account_hash}/module/{module_name}

This endpoint fetches detailed information about a single smart contract (module) deployed under a specific Aptos account. The response contains the module's bytecode and ABI, providing a complete description of its functions, structs, and on-chain logic.

---

## Supported Network
- Mainnet

---

## Parameters

| Parameter        | Data type | Description                                                                 | Required | In    |
|------------------|-----------|-----------------------------------------------------------------------------|----------|-------|
| `account_hash`   | string    | Aptos account address.                                                      | Yes      | Path  |
| `module_name`    | string    | The name of the smart contract module.                                      | Yes      | Path  |
| `ledger_version` | string    | The ledger version to retrieve the module state. Optional parameter.        | No       | Query |

---

## Request

**Base URL**
```bash
https://go.getblock.io/<ACCESS_TOKEN>/
```

**Example (cURL)**
```bash
curl --location 'https://go.getblock.io/<ACCESS_TOKEN>/v1/accounts/0x190d44266241744264b964a37b8f09863167a12d3e70cda39376cfb4e3561e12/module/curves'
```

### Response 

```json
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
        "generic_type_params": [{ "constraints": [] }],
        "params": [],
        "return": []
      },
      {
        "name": "is_stable",
        "visibility": "public",
        "is_entry": false,
        "is_view": false,
        "generic_type_params": [{ "constraints": [] }],
        "params": [],
        "return": ["bool"]
      },
      {
        "name": "is_uncorrelated",
        "visibility": "public",
        "is_entry": false,
        "is_view": false,
        "generic_type_params": [{ "constraints": [] }],
        "params": [],
        "return": ["bool"]
      },
      {
        "name": "is_valid_curve",
        "visibility": "public",
        "is_entry": false,
        "is_view": false,
        "generic_type_params": [{ "constraints": [] }],
        "params": [],
        "return": ["bool"]
      }
    ],
    "structs": [
      {
        "name": "Stable",
        "is_native": false,
        "is_event": false,
        "abilities": [],
        "generic_type_params": [],
        "fields": [{ "name": "dummy_field", "type": "bool" }]
      },
      {
        "name": "Uncorrelated",
        "is_native": false,
        "is_event": false,
        "abilities": [],
        "generic_type_params": [],
        "fields": [{ "name": "dummy_field", "type": "bool" }]
      }
    ]
  }
}
```
---

## Response Parameter Definition

| Field                   | Type   | Description                                                                    |
| ----------------------- | ------ | ------------------------------------------------------------------------------ |
| `bytecode`              | string | Hex-encoded bytecode of the deployed module.                                   |
| `abi`                   | object | ABI (Application Binary Interface) containing metadata for module interaction. |
| `abi.address`           | string | Account address where the module is deployed.                                  |
| `abi.name`              | string | Name of the module.                                                            |
| `abi.exposed_functions` | array  | List of public functions, including parameters, return types, and visibility.  |
| `abi.structs`           | array  | Struct definitions with fields and type information.                           |

---

### Use Cases

This endpoint can be used to:

- Retrieve module ABIs for building smart contract interaction interfaces.

- Fetch module bytecode for on-chain security analysis and verification.

- Display contract details in analytics dashboards or blockchain explorers.

- Enable dApps to validate if a specific module exists before user interaction.

## Code Examples

**Python (requests)**

```python
import requests

url = "https://go.getblock.io/<ACCESS_TOKEN>/v1/accounts/0x190d44266241744264b964a37b8f09863167a12d3e70cda39376cfb4e3561e12/module/curves"

response = requests.get(url)

print(response.text)
```

**Node(Axios)**

```js
import axios from 'axios';

let config = {
  method: 'get',
  maxBodyLength: Infinity,
  url: 'https://go.getblock.io/<ACCESS_TOKEN>/v1/accounts/0x190d44266241744264b964a37b8f09863167a12d3e70cda39376cfb4e3561e12/module/curves'
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

## Error Handling

| Status Code | Error Message         | Cause                                           |
| ----------- | --------------------- | ----------------------------------------------- |
| **403**     | Forbidden             | Missing or invalid `<ACCESS_TOKEN>`.            |
| **404**     | Resource not found    | The specified module does not exist.            |
| **500**     | Internal server error | Node or network issue. Retry the request later. |

---

## Integration with Web3

By integrating `/v1/accounts/{account_hash}/module/{module_name}`, developers can:

- Retrieve module ABIs to construct transactions for smart contracts dynamically.

- Validate wallet or dApp interactions by confirming module existence.

- Fetch module bytecode for security inspection or automated audits.

- Allow services or other contracts to interpret available structs and functions dynamically.
