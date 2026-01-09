---
description: >-
  GetBlock provides fast and reliable access to Sui RPC nodes via JSON-RPC API.
  Connect to the SUI network without running your own infrastructure.
---

# Sui (SUI)

S is a high-performance Layer 1 blockchain designed for low-latency, high-throughput applications. Built by Mysten Labs using the Move programming language, SUI features a unique object-centric data model that enables parallel transaction execution. The network achieves sub-second finality through its innovative Narwhal and Bullshark (transitioning to Mysticeti) consensus mechanism, making it ideal for gaming, DeFi, and real-time applications.

### Key Features

* **Object-Centric Model**: Unique architecture where everything is an object with ownership properties
* **Move Programming Language**: Rust-based, asset-oriented smart contract language with built-in safety
* **Parallel Transaction Execution**: Independent transactions processed simultaneously for high throughput
* **Sub-Second Finality**: \~400ms average transaction finality with Mysticeti consensus
* **Horizontal Scalability**: Designed for 100,000+ TPS through parallel processing
* **SUI Native Token**: Uses SUI for gas fees, staking, and governance (9 decimals, smallest unit: MIST)
* **Dynamic NFTs**: Native support for evolving, composable digital assets
* **Sponsored Transactions**: Gas payment abstraction for better user experience
* **SuiNS**: Native name service for human-readable addresses

{% hint style="info" %}
_TECHNICAL DISCLAIMER: AUTHORITATIVE JSON-RPC API SPECIFICATION._

_GetBlock's RPC API reference documentation is provided exclusively for informational purposes and streamlined developer experience optimization. The canonical and normative specification for SUI JSON-RPC methods is solely maintained and published through the official SUI documentation portal at_ [_docs.sui.io_](https://docs.sui.io/)_. This resource constitutes the sole authoritative reference implementation of the JSON-RPC 2.0 protocol interface for SUI network clients._
{% endhint %}

## Network Information

| Property        | Value                               |
| --------------- | ----------------------------------- |
| Network Name    | SUI Mainnet                         |
| Chain ID        | 101                                 |
| Native Currency | SUI                                 |
| Decimals        | 9 (1 SUI = 1,000,000,000 MIST)      |
| RPC URL         | https://fullnode.mainnet.sui.io:443 |
| Consensus       | Narwhal & Mysticeti                 |
| Block Time      | Sub-second (\~400ms finality)       |
| Smart Contracts | Move Language                       |

## Base URL

{% tabs %}
{% tab title="Frankfurt, Germany" %}
```bash
https://go.getblock.io
```
{% endtab %}

{% tab title="New York, USA" %}
```bash
https://go.getblock.us
```
{% endtab %}
{% endtabs %}

## Supported Networks

| Network | Chain ID | JSON RPC |
| ------- | -------- | -------- |
| Mainnet | 101      | ✅        |

## Quickstart

In this section, you will learn how to make your first call with either:

* Axios
* Python

{% hint style="warning" %}
Before you begin, you must have already installed `npm` or `yarn` on your local machine (for the Axios example) or Python and pip (for the Python example).

Refer to:

* [npm](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm)
* [yarn](https://classic.yarnpkg.com/lang/en/docs/install)
{% endhint %}

{% stepper %}
{% step %}
### Quickstart with Axios

#### 1. Set up the project

Create and initialize a new project:

```bash
mkdir sui-api-quickstart
cd sui-api-quickstart
npm init --yes
```

#### Install Axios

```bash
npm install axios
```

#### 2. Create file

Create a new file named `index.js`. This is where you will make your first call.

#### 3. Set ES module type

Set the ES module `"type": "module"` in your `package.json`.

#### 4. Add code

Add the following code to `index.js`:

{% code title="index.js" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "sui_getLatestCheckpointSequenceNumber",
    "params": [],
    "id": "getblock.io"
});

const config = {
    method: 'post',
    url: 'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers: {
        'Content-Type': 'application/json'
    },
    data: data
};

axios(config)
    .then(response => console.log(JSON.stringify(response.data)))
    .catch(error => console.log(error));
```
{% endcode %}

Replace `<ACCESS-TOKEN>` with your actual access token from GetBlock.

#### 5. Run the script

```bash
node index.js
```

Expected output (example):

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "231610955"
}
```
{% endstep %}

{% step %}
### Quickstart with Python and Requests

#### 1. Set up the project directory

```bash
mkdir sui-api-quickstart
cd sui-api-quickstart
```

#### 2. Create and activate a virtual environment

```bash
python -m venv venv
source venv/bin/activate

# On Windows, use venv\Scripts\activate
```

#### 3. Install requests

```bash
pip install requests
```

#### 4. Create script

Create a file called `main.py` with the following content:

{% code title="main.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "sui_getLatestCheckpointSequenceNumber",
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

#### 5. Run the script

```bash
python main.py
```
{% endstep %}
{% endstepper %}

## Available API Methods

GetBlock provides access to standard SUI JSON-RPC methods for the SUI network.

#### Coin Query Methods

| Method                | Description                                |
| --------------------- | ------------------------------------------ |
| suix\_getBalance      | Returns balance for a specific coin type   |
| suix\_getAllBalances  | Returns all coin balances for an address   |
| suix\_getAllCoins     | Returns all coin objects owned by address  |
| suix\_getCoins        | Returns coins of specific type for address |
| suix\_getCoinMetadata | Returns coin metadata (symbol, decimals)   |
| suix\_getTotalSupply  | Returns total supply for a coin type       |

#### Read Methods

| Method                                 | Description                           |
| -------------------------------------- | ------------------------------------- |
| sui\_getObject                         | Returns object data by ID             |
| sui\_multiGetObjects                   | Returns multiple objects by IDs       |
| sui\_getTransactionBlock               | Returns transaction by digest         |
| sui\_multiGetTransactionBlocks         | Returns multiple transactions         |
| sui\_getChainIdentifier                | Returns chain genesis identifier      |
| sui\_getCheckpoint                     | Returns checkpoint by sequence number |
| sui\_getCheckpoints                    | Returns paginated checkpoints         |
| sui\_getLatestCheckpointSequenceNumber | Returns latest checkpoint number      |
| sui\_getTotalTransactionBlocks         | Returns total transaction count       |
| sui\_getEvents                         | Returns events for a transaction      |
| sui\_getProtocolConfig                 | Returns protocol configuration        |
| sui\_tryGetPastObject                  | Returns object at specific version    |
| sui\_tryMultiGetPastObjects            | Returns multiple past objects         |

#### Extended Query Methods

| Method                          | Description                       |
| ------------------------------- | --------------------------------- |
| suix\_getOwnedObjects           | Returns objects owned by address  |
| suix\_getDynamicFields          | Returns dynamic fields of object  |
| suix\_getDynamicFieldObject     | Returns specific dynamic field    |
| suix\_queryEvents               | Queries events with filters       |
| suix\_queryTransactionBlocks    | Queries transactions with filters |
| suix\_resolveNameServiceAddress | Resolves SuiNS name to address    |
| suix\_resolveNameServiceNames   | Resolves address to SuiNS names   |

#### Governance Methods

| Method                        | Description                      |
| ----------------------------- | -------------------------------- |
| suix\_getCommitteeInfo        | Returns validator committee info |
| suix\_getLatestSuiSystemState | Returns system state object      |
| suix\_getReferenceGasPrice    | Returns current gas price        |
| suix\_getStakes               | Returns stakes for an address    |
| suix\_getStakesByIds          | Returns stakes by object IDs     |
| suix\_getValidatorsApy        | Returns validator APY data       |

#### Move Utils Methods

| Method                                 | Description                     |
| -------------------------------------- | ------------------------------- |
| sui\_getMoveFunctionArgTypes           | Returns function argument types |
| sui\_getNormalizedMoveFunction         | Returns function definition     |
| sui\_getNormalizedMoveModule           | Returns module definition       |
| sui\_getNormalizedMoveModulesByPackage | Returns all modules in package  |
| sui\_getNormalizedMoveStruct           | Returns struct definition       |

#### Write Methods

| Method                          | Description                     |
| ------------------------------- | ------------------------------- |
| sui\_executeTransactionBlock    | Executes signed transaction     |
| sui\_dryRunTransactionBlock     | Simulates transaction execution |
| sui\_devInspectTransactionBlock | Dev-mode transaction testing    |

#### Subscription Methods (WebSocket)

| Method                     | Description                      |
| -------------------------- | -------------------------------- |
| suix\_subscribeEvent       | Subscribes to event stream       |
| suix\_subscribeTransaction | Subscribes to transaction stream |

## Support

For technical support and questions:

* Support: [support@getblock.io](mailto:support@getblock.io)

## See Also

* [Sui Developer Documentation](https://docs.sui.io/)
* [Sui Block Explorer - SuiScan](https://suiscan.xyz/)
* [Sui Block Explorer - SuiVision](https://suivision.xyz/)
* [Sui Bridge](https://bridge.sui.io/)
* [Sui TypeScript SDK](https://sdk.mystenlabs.com/typescript)
* [Sui Rust SDK](https://docs.sui.io/guides/developer/sui-101/building-with-the-sui-rust-sdk)
* [Move Language Documentation](https://move-language.github.io/move/)
