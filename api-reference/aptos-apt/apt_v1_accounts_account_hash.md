---
description: >-
    Example code for the /v1/accounts/{account_hash} json-rpc method. Сomplete guide on how to use /v1/accounts/{account_hash} json-rpc in GetBlock.io Web3 documentation.
---

# /v1/accounts/{account\_hash} - Aptos

This endpoint retrieves account details, such as the number of transactions submitted by an account and its authentication key.

## Supported Networks

* Mainnet
    

## Parameters

| Parameter | Data type | Description | Required | In |
| --- | --- | --- | --- | --- |
| `account_hash` | string | Aptos account address | Yes | Path |

---

## Request Example

**URL**

```bash
https://go.getblock.io/<ACCESS-TOKEN>/
```

**Example**

```bash
curl --location 'https://go.getblock.io/<ACCESS_TOKEN>/v1/accounts/0xbf9239be9eb7e7a3d8e4c1f36083464fd47e6bd1f82a43b7c0f7ee958705a52f'
```

## Response

```json
{
  "sequence_number": "234541",
  "authentication_key": "0xbf9239be9eb7e7a3d8e4c1f36083464fd47e6bd1f82a43b7c0f7ee958705a52f"
}
```

## Response Definition

| Value | Type | Description |
| --- | --- | --- |
| `sequence_number` | string | The number of transactions that have been submitted and committed from the account. |
| `authentication_key` | string | The authentication key associated with the account, in hex-encoded format. |

## Use Cases

This method can be used for:

* Wallets to fetch sequence numbers before sending transactions (to prevent replay attacks).
    
* Creating an account overview page or profile in a dApp.
    
* Validating if an account exists on-chain to avoid sending assets to the wrong account.
    

## Code Examples

### Python (requests)

```python
import requests

url = "https://go.getblock.io/<ACCESS_TOKEN>/v1/accounts/0xbf9239be9eb7e7a3d8e4c1f36083464fd47e6bd1f82a43b7c0f7ee958705a52f"

response = requests.get(url)

print(response.text)
```

### Node.js (Axios)

```js
import axios from 'axios';

let config = {
  method: 'get',
  maxBodyLength: Infinity,
  url: 'https://go.getblock.io/<ACCESS_TOKEN>/v1/accounts/0xbf9239be9eb7e7a3d8e4c1f36083464fd47e6bd1f82a43b7c0f7ee958705a52f',
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

You may encounter responses like:

```json
{
  "sequence_number": "0",
  "authentication_key": "0x00000000000000000000000bf9239be9eb7e7a3d8e4c1f36083464fd47e6bd1f"
}
```

This means the account hash is incorrect or the account does not exist on-chain at the current ledger state.

* **403** → Access token is missing.
    

## Integration with Web3

By integrating `/v1/accounts/{account_hash}`, developers can:

* Fetch sequence numbers for transaction signing.
    
* Validate account existence and state (useful for onboarding users).
    
* Support wallet and dApp account management.
    
* Enable reliable transaction pipelines.
