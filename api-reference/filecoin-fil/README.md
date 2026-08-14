---
description: >-
  GetBlock provides fast and reliable access to Filecoin nodes via JSON-RPC API.
  Connect to the Filecoin network without running your own infrastructure.
---

# Filecoin (FIL)

Filecoin is a decentralized storage network whose blockchain coordinates a market for verifiable data storage, secured by Proof-of-Replication and Proof-of-Spacetime. The native token **FIL** pays for storage and gas fees; its base unit is the attoFIL (1 FIL = 10^18 attoFIL). Blocks are produced in \~30-second epochs, and the chain state is organized around tipsets and content-addressed CIDs.

GetBlock exposes Filecoin through two JSON-RPC surfaces:

* The **native Lotus API** (`Filecoin.*` methods) for chain, state, wallet, message pool, and gas operations.
* The **FEVM API** (`eth_*` methods) for the Filecoin EVM, an EVM-compatible runtime with chain ID `314`.

## Interfaces

| Interface      | Methods             | Use it for                                                               |
| -------------- | ------------------- | ------------------------------------------------------------------------ |
| Native (Lotus) | `Filecoin.<Method>` | Filecoin-native data: tipsets, actors, miners, messages, wallet balances |
| FEVM (EVM)     | `eth_*`             | EVM smart contracts on the Filecoin EVM (Solidity, ethers.js, viem)      |

Both surfaces are served over JSON-RPC 2.0 at the same endpoint; the method name selects the surface. Native methods use Filecoin address forms (`f0`/`f1`/`f2`/`f3`/`f4`) and CIDs; the FEVM layer uses `0x` addresses and standard Ethereum types. An `f4` address and its `0x` form refer to the same account across the two surfaces.

## Key Features

* **Decentralized Storage**: A market for verifiable storage secured by Proof-of-Replication and Proof-of-Spacetime
* **Native Lotus API**: `Filecoin.*` methods for tipsets, actors, miners, messages, and wallet balances
* **FEVM (EVM Compatibility)**: Deploy Solidity contracts on the Filecoin EVM at chain ID 314 with standard `eth_*` methods
* **Tipset Chain Structure**: Blocks are grouped into tipsets per epoch; state is read against a tipset key
* **Content Addressing**: Chain objects are identified by CIDs, integrating with IPFS content addressing
* **FIL Native Token**: FIL pays for storage and gas; the base unit is attoFIL (1 FIL = 10^18 attoFIL)
* **Actor Model**: Accounts, miners, and system components are actors with code and state

{% hint style="info" %}
GetBlock's Filecoin API reference documentation is provided for informational purposes and developer experience. The canonical specifications are maintained by the Filecoin project: the Lotus JSON-RPC reference at [docs.filecoin.io](https://docs.filecoin.io/reference/json-rpc), and the Ethereum JSON-RPC API for the FEVM. Amounts are in attoFIL.
{% endhint %}

## Network Information

<table data-search="false"><thead><tr><th>Property</th><th>Value</th></tr></thead><tbody><tr><td>Network Name</td><td>Filecoin Mainnet</td></tr><tr><td>Native Currency</td><td>FIL (1 FIL = 10^18 attoFIL)</td></tr><tr><td>FEVM Chain ID</td><td>314 (<code>0x13a</code>)</td></tr><tr><td>Epoch (Block Time)</td><td>~30 seconds</td></tr><tr><td>Consensus</td><td>Expected Consensus (PoRep and PoSt)</td></tr><tr><td>Native API</td><td>Lotus JSON-RPC (<code>Filecoin.&#x3C;Method></code>)</td></tr><tr><td>EVM Layer</td><td>FEVM (EVM-compatible, <code>eth_*</code>)</td></tr><tr><td>Address Formats</td><td>Filecoin (<code>f0</code>/<code>f1</code>/<code>f2</code>/<code>f3</code>/<code>f4</code>) and <code>0x</code> (FEVM)</td></tr><tr><td>Block Explorer</td><td><a href="https://filfox.info/">Filfox</a></td></tr><tr><td>Testnet</td><td>Calibration (FEVM Chain ID 314159)</td></tr></tbody></table>

## Base URL

```bash
https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
```

Both the native and FEVM methods are called by sending a `POST` request with a JSON-RPC 2.0 body to the endpoint. Replace `<ACCESS-TOKEN>` with the access token from the GetBlock dashboard.

## Supported Networks

| Network | Native (Lotus) | FEVM (eth\_) | Websocket | Frankfurt, Germany |
| ------- | -------------- | ------------ | --------- | ------------------ |
| Mainnet | ✅              | ✅            | ✅         | ✅                  |

Calibration Testnet (FEVM Chain ID `314159`) is the test environment for staging before mainnet. Confirm testnet endpoint availability from the GetBlock dashboard.\
<br>

## Quickstart

In this section, you will learn how to make your first call with either:

* **Axios** (JavaScript / Node.js)
* **Python** (Requests library)

Before you begin, you must have already installed [`npm`](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm) or [`yarn`](https://classic.yarnpkg.com/lang/en/docs/install) on your local machine (for the Axios example) or Python and pip (for the Python example).

{% tabs %}
{% tab title="JavaScript (Axios)" %}
{% stepper %}
{% step %}
#### Setup project

Create and initialize a new project:

```bash
mkdir polkadot-api-quickstart
cd polkadot-api-quickstart
npm init --yes
```
{% endstep %}

{% step %}
#### Install Axios

```bash
npm install axios
```
{% endstep %}

{% step %}
#### Create file

Create a new file named `index.js`. This is where you will make your first call.
{% endstep %}

{% step %}
#### Set ES module type

Set the ES module `"type": "module"` in your `package.json`.
{% endstep %}

{% step %}
#### Add code

Add the following code to `index.js`:

{% code title="index.js" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "eth_chainId",
    "params": [],
    "id": "getblock.io"
});

const config = {
    method: 'post',
    url: 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/',
    headers: {
        'Content-Type': 'application/json'
    },
    data: data
};

axios(config)
    .then(response => console.log(JSON.stringify(response.data, null, 2)))
    .catch(error => console.log(error));
```
{% endcode %}

Replace `<ACCESS-TOKEN>` with your actual GetBlock access token.
{% endstep %}

{% step %}
#### Run the script

```bash
node index.js
```

Expected output:

```json
{
    "jsonrpc": "2.0",
    "result": "0x13a",
    "id": "getblock.io"
}
```

The `result` field is the Kaia Mainnet chain ID (`0x2019` = 8217 in decimal). This confirms you are connected to the correct network.
{% endstep %}
{% endstepper %}
{% endtab %}

{% tab title="Python (Requests)" %}
{% stepper %}
{% step %}
#### Setup the project directory

```bash
mkdir polkadot-api-quickstart
cd polkadot-api-quickstart
```
{% endstep %}

{% step %}
#### Create and activate a virtual environment

```bash
python -m venv venv
source venv/bin/activate

# On Windows, use venv\Scripts\activate
```
{% endstep %}

{% step %}
#### Install requests

```bash
pip install requests
```
{% endstep %}

{% step %}
#### Create script

Create a file called `main.py` with the following content:

{% code title="main.py" %}
```python
import requests
import json

url = "https://shared.eu-central-1.getblock.io/ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "eth_chainId",
    "params": [],
    "id": "getblock.io"
})

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.text)
```
{% endcode %}

Replace `<ACCESS-TOKEN>` with your actual access token from GetBlock.
{% endstep %}

{% step %}
#### Run the script

```bash
python main.py
```
{% endstep %}
{% endstepper %}
{% endtab %}
{% endtabs %}

## APIs

* [Native API (Lotus)](native-api/) — Filecoin-native `Filecoin.*` methods
* [FEVM API (EVM) ](fevm-api/)— Ethereum-compatible `eth_*` methods

## Support

For technical support and questions:

* Support: [support@getblock.io](mailto:support@getblock.io)

## See Also

* [Filecoin Documentation](https://docs.filecoin.io/)
* [Lotus JSON-RPC Reference](https://docs.filecoin.io/reference/json-rpc)
* [Filfox Explorer](https://filfox.info/)
