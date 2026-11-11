---
description: >-
  GetBlock provides fast and reliable access to Flow nodes via REST API. Connect
  to the Flow network without running your own infrastructure.
---

# Flow

Flow is a Proof-of-Stake Layer-1 blockchain designed for consumer applications, games, and digital assets, with a multi-role node architecture that separates collection, consensus, execution, and verification for scale without sharding. Smart contracts are written in Cadence, a resource-oriented language in which assets are first-class values that cannot be copied or lost. The native token is FLOW (1 FLOW = 100,000,000 units), and accounts are identified by 16-hex-character addresses. GetBlock exposes Flow through the Access API over REST (HTTP + JSON): a path-addressed interface where reads use GET and script execution and transaction submission use POST.

### Key Features

* **Consumer-Scale L1**: Multi-role architecture (collection / consensus / execution / verification) scales without sharding
* **Cadence Contracts**: Resource-oriented smart contracts where digital assets are first-class, safe-by-construction values
* **REST Access API**: The Access node exposes a path-addressed REST API — no JSON-RPC envelope
* **Base64 Payloads**: Cadence scripts, arguments, and results are carried as base64-encoded JSON-Cadence
* **Block-Pinned Reads**: Accounts, scripts, and blocks accept a height or the keywords final / sealed for historical state
* **Sealed Finality**: A transaction is final once its block is Sealed

{% hint style="info" %}
_TECHNICAL DISCLAIMER: AUTHORITATIVE REST API SPECIFICATION._

_GetBlock's API reference documentation is provided exclusively for informational purposes and to optimize the developer experience. The canonical specification is the Flow Access HTTP API, documented at_ [_developers.flow.com_](https://developers.flow.com/)_. This package covers the Cadence Access API; Flow also offers a separate EVM-equivalent JSON-RPC interface (Flow EVM), which is documented separately._
{% endhint %}

### Network Information

| Property          | Value                                       |
| ----------------- | ------------------------------------------- |
| Network Name      | Flow Mainnet                                |
| Chain ID          | flow-mainnet                                |
| Native Currency   | FLOW (1 FLOW = 100,000,000 units)           |
| Consensus         | Proof-of-Stake (multi-role)                 |
| Contract Language | Cadence                                     |
| Address Format    | 16 hex characters (e.g. 0x1654653399040a61) |
| Finality          | Sealed blocks                               |

### Base URL

{% tabs %}
{% tab title="Frankfurt, Germany" %}
```
https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}

{% tab title="New York, USA" %}
```
https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}

{% tab title="Singapore, Singapore" %}
```
https://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}
{% endtabs %}

### Making Requests

The Flow Access API is REST, not JSON-RPC: the resource is chosen by the URL path appended to your GetBlock endpoint. Reads are GET; executing a Cadence script and submitting a transaction are POST with a JSON body. Cadence code, arguments, and results travel as base64-encoded JSON-Cadence. Many reads accept `final` or `sealed` in place of a block height.

{% code overflow="wrap" %}
```bash
export FLOW_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

# Latest sealed block
curl "${FLOW_REST}v1/blocks?height=sealed"

# Account balance, keys, and contracts
curl "${FLOW_REST}v1/accounts/0x1654653399040a61?expand=keys,contracts"
```
{% endcode %}

## Quickstart

{% tabs %}
{% tab title="Javascript(Axios)" %}
{% stepper %}
{% step %}
### Setup project

```bash
mkdir flow-api-quickstart && cd flow-api-quickstart && npm init --yes
```
{% endstep %}

{% step %}
### Install dependency

```bash
npm install axios
```
{% endstep %}

{% step %}
### Add code

{% code title="index.js" %}
```javascript
const axios = require('axios');

const url = 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/';

axios.get(`${url}v1/blocks`, {
  params: { height: 'sealed' },
  headers: { 'Content-Type': 'application/json' }
})
.then(response => {
  console.log('Latest sealed block:', response.data[0].header.height);
})
.catch(error => console.error(error));
```
{% endcode %}
{% endstep %}

{% step %}
### Run the script

```bash
node index.js
```
{% endstep %}

{% step %}
### Response

{% code overflow="wrap" %}
```bash
Latest sealed block: 88226300
```
{% endcode %}
{% endstep %}
{% endstepper %}
{% endtab %}

{% tab title="Python(Request)" %}
{% stepper %}
{% step %}
### Setup project

```bash
mkdir flow-api-quickstart && cd flow-api-quickstart
python -m venv venv && source venv/bin/activate
```
{% endstep %}

{% step %}
### Install dependency

```bash
pip install requests
```
{% endstep %}

{% step %}
### Add code

{% code title="main.py" %}
```python
import requests

url = "https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/"

params = {
    "height": "sealed"
}

headers = {
    'Content-Type': 'application/json'
}

response = requests.get(url + "v1/blocks", headers=headers, params=params)
print(response.text)
```
{% endcode %}
{% endstep %}

{% step %}
### Run the script

```bash
python main.py
```
{% endstep %}
{% endstepper %}
{% endtab %}
{% endtabs %}

## Available REST Endpoints

### Chain & Node Info

| Endpoint           | Method | Description                                                                                                                    |
| ------------------ | ------ | ------------------------------------------------------------------------------------------------------------------------------ |
| node-version-info  | GET    | Returns version information about the Access node serving the request, including the protocol version, spork ID, and node role |
| network-parameters | GET    | Returns the network parameters of the connected Flow network, chiefly the chain identifier (flow-mainnet, flow-testnet, …)     |

### Blocks

| Endpoint    | Method | Description                            |
| ----------- | ------ | -------------------------------------- |
| blocks      | GET    | Returns one or more blocks by height   |
| block-by-id | GET    | Returns one or more blocks by block ID |

### Accounts

| Endpoint    | Method | Description                                                                                                                                              |
| ----------- | ------ | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| account     | GET    | Returns a Flow account: its FLOW balance, public keys, and deployed Cadence contracts                                                                    |
| account-key | GET    | Returns a single public key of an account by its key index, including its signing and hashing algorithms, weight, sequence number, and revocation status |

### Collections & Transactions

| Endpoint           | Method | Description                                                                                                                                  |
| ------------------ | ------ | -------------------------------------------------------------------------------------------------------------------------------------------- |
| collection-by-id   | GET    | Returns a collection by ID                                                                                                                   |
| transaction        | GET    | Returns a transaction by ID: its Cadence script, arguments, reference block, gas limit, proposer/payer/authorizers, and signatures           |
| transaction-result | GET    | Returns the execution result of a transaction by its ID: status, status code, any error message, computation used, and the events it emitted |
| send-transaction   | POST   | Submits a signed transaction to the network and returns the created transaction, including its ID                                            |

### Scripts & Events

| Endpoint       | Method | Description                                                                                                    |
| -------------- | ------ | -------------------------------------------------------------------------------------------------------------- |
| execute-script | POST   | Executes a read-only Cadence script against the chain state and returns the base64-encoded JSON-Cadence result |
| events         | GET    | Returns events of a given type within a block-height range, or across a set of block IDs                       |

### Execution Results

| Endpoint               | Method | Description                                                                                                                 |
| ---------------------- | ------ | --------------------------------------------------------------------------------------------------------------------------- |
| execution-results      | GET    | Returns the execution results for one or more blocks, selected by block ID                                                  |
| execution-result-by-id | GET    | Returns a single execution result by its own result ID (not the block ID), including its chunks and the block it commits to |

## Support

For technical support and questions:

* Support: [support@getblock.io](mailto:support@getblock.io)

## See Also

* [Flow Access HTTP API](https://developers.flow.com/)
* [Cadence Language Documentation](https://cadence-lang.org/)
* [Flowscan Explorer](https://www.flowscan.io/)
* [Flow Website](https://flow.com/)
