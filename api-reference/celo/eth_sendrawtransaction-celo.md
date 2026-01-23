---
description: >-
  Example code for the eth_newFilter JSON-RPC method. Complete guide on how to
  use eth_newFilter JSON-RPC in GetBlock Web3 documentation.
---

# eth\_sendRawTransaction - Celo

The `eth_sendRawTransaction` method submits a pre-signed transaction to the Celo network for execution. This is the primary method for broadcasting transactions. With Celo's one-block finality and sub-cent fees, transactions are confirmed rapidly and economically. Celo also supports fee abstraction, allowing gas to be paid in stablecoins.

## Parameters

| Parameter             | Type   | Required | Description                                 |
| --------------------- | ------ | -------- | ------------------------------------------- |
| signedTransactionData | string | Yes      | The signed transaction data as a hex string |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code title="curl" overflow="wrap" %}
```bash
curl -X POST https://go.getblock.io/<ACCESS-TOKEN>/ \
-H "Content-Type: application/json" \
-d '{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "method": "eth_sendRawTransaction",
  "params": ["0xf86c098504a817c800825208943535353535353535353535353535353535353535880de0b6b3a76400008025a028ef61340bd939bc2195fe537567866003e1a15d3c71ff63e1590620aa636276a067cbe9d8997f761aecb703304b3800ccf555c9f3dc64214b297fb1966a3b6d83"]
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
  jsonrpc: '2.0',
  id: 'getblock.io',
  method: 'eth_sendRawTransaction',
  params: ['0xf86c098504a817c800825208943535353535353535353535353535353535353535880de0b6b3a76400008025a028ef61340bd939bc2195fe537567866003e1a15d3c71ff63e1590620aa636276a067cbe9d8997f761aecb703304b3800ccf555c9f3dc64214b297fb1966a3b6d83']
};

axios.post(url, payload, {
  headers: { 'Content-Type': 'application/json' }
})
.then(response => console.log(response.data))
.catch(error => console.error(error));
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "method": "eth_sendRawTransaction",
    "params": ["0xf86c098504a817c800825208943535353535353535353535353535353535353535880de0b6b3a76400008025a028ef61340bd939bc2195fe537567866003e1a15d3c71ff63e1590620aa636276a067cbe9d8997f761aecb703304b3800ccf555c9f3dc64214b297fb1966a3b6d83"]
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Example

{% code title="response.json" %}
```json
{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": "0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b"
}
```
{% endcode %}

## Response Definition

| Field  | Type   | Description              |
| ------ | ------ | ------------------------ |
| result | string | 32-byte transaction hash |

## Use Cases

* Submit signed transactions to the network
* Deploy smart contracts
* Transfer CELO tokens
* Execute contract functions
* Send stablecoin payments (cUSD, cEUR)

## Error Handling

| Error Code | Description                                             |
| ---------- | ------------------------------------------------------- |
| -32602     | Invalid params - malformed transaction data             |
| -32603     | Internal error - node processing issues                 |
| -32000     | Transaction rejected - insufficient funds, nonce issues |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');
const wallet = new ethers.Wallet('YOUR_PRIVATE_KEY', provider);

const tx = await wallet.sendTransaction({
  to: '0x8626f6940E2eb28930eFb4CeF49B2d1F2C9C1199',
  value: ethers.parseEther('1.0')
});
console.log(tx.hash);
```
{% endcode %}
{% endtab %}

{% tab title="ContractKit" %}
{% code title="contractkit-example.js" %}
```javascript
const { newKit } = require('@celo/contractkit');

const kit = newKit('https://go.getblock.io/<ACCESS-TOKEN>/');
kit.addAccount('YOUR_PRIVATE_KEY');

const tx = await kit.sendTransaction({
  to: '0x8626f6940E2eb28930eFb4CeF49B2d1F2C9C1199',
  value: kit.web3.utils.toWei('1', 'ether')
});
console.log(tx.transactionHash);
```
{% endcode %}
{% endtab %}
{% endtabs %}
