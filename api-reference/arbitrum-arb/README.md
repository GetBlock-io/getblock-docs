---
description: >-
  Arbitrum Network API Reference for efficient interaction with ARB nodes,
  enabling fast, low-cost Layer 2 scaling solutions for Ethereum with high
  throughput and secure smart contracts.
---

# Arbitrum (ARB)

### Overview

Arbitrum is a suite of Ethereum Layer 2 scaling technologies built on the Arbitrum Nitro tech stack that includes Arbitrum One (a live implementation of the Arbitrum Rollup Protocol) and Arbitrum Nova (a live implementation of the Arbitrum AnyTrust Protocol).

Arbitrum chains are EVM-compatible blockchains that use an underlying EVM chain (e.g., Ethereum) for settlement and succinct fraud proofs (as needed). You can use Arbitrum chains to do everything you do on Ethereum—use Web3 apps, deploy smart contracts, etc., but your transactions will be cheaper and faster.

#### Key Features

1. **High Throughput & Low Fees:** Arbitrum offers faster transaction speeds and significantly lower fees than the Ethereum mainnet, while maintaining EVM support, making it ideal for high-volume applications like DeFi or gaming.
2. **EVM Compatibility:** Arbitrum chains are Ethereum-compatible and allow you to deploy Solidity smart contracts, as well as Vyper and other languages that compile to EVM bytecode.&#x20;
3. **Extended Language Support (Stylus):** The latest version of the Arbitrum tech stack, called [Stylus](https://docs.arbitrum.io/stylus/quickstart), maintains Nitro's Ethereum compatibility while enabling the development of highly performant smart contracts in programming languages such as Rust, C++, and C.&#x20;
4. **Fraud-Proof Security:** The Arbitrum Rollup Protocol is a trustless, permissionless protocol that uses its underlying base layer for data availability and inherits its security.&#x20;
5. **Cross-Chain Interactions:** Every Arbitrum chain includes a bridge to/from its parent chain. Arbitrum enables easy cross-chain interactions, asset transfers, and data exchange with Ethereum and other EVM-compatible networks, fostering a connected ecosystem for multi-chain dApps.

{% hint style="info" %}
_TECHNICAL DISCLAIMER: AUTHORITATIVE JSON-RPC API SPECIFICATION._&#x20;

_GetBlock's RPC API reference documentation is provided exclusively for informational purposes and to optimize the developer experience. The canonical and normative specification for Ethereum Virtual Machine (EVM) JSON-RPC methods is solely maintained and published through the official Ethereum Foundation documentation portal at_ [_ethereum.org_](http://ethereum.org/)_. This resource constitutes the sole authoritative reference implementation of the JSON-RPC 2.0 protocol interface across EVM-compatible execution clients._
{% endhint %}

### Supported Network&#x20;

| Network | Chain ID | JSON | WSS |
| ------- | -------- | ---- | --- |
| Mainnet | 42161    | ✔    | ✔   |
| Sepolia | 421614   | ✔    | ✔   |
| Nova    | 42170    | ✔    | ✔   |

### Base URL

{% tabs %}
{% tab title="New York, USA" %}
```bash
https://go.getblock.us/
```
{% endtab %}

{% tab title="Franfurt, Germany" %}
```bash
https://go.getblock.io
```
{% endtab %}
{% endtabs %}

### Quickstart

In this section, you will learn how to make your first call with either:

* Axios
* Python

#### Quickstart with Axios

Before you begin, you must have already installed `npm` or `yarn` on your local machine. If not, check out [npm](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm) or [yarn](https://classic.yarnpkg.com/lang/en/docs/install).

1. Set up your project using this command:

{% tabs %}
{% tab title="npm" %}
```bash
mkdir arbitrum-api-quickstart
cd arbitrum-api-quickstart
npm init --yes
```
{% endtab %}

{% tab title="yarn" %}
```bash
mkdir arbitrum-api-quickstart
cd arbitrum-api-quickstart
yarn init --yes
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
This creates a project directory named `aptos-api-quickstart` and initialises a Node.js project within it.
{% endhint %}

2. Install Axios using this command:

{% tabs %}
{% tab title="npm" %}
```bash
npm install axios
```
{% endtab %}

{% tab title="yarn" %}
```bash
yarn add axios
```
{% endtab %}
{% endtabs %}

3. Create a new file and name it `index.js`. This is where you will make your first call.
4. Set the ES module `"type": "module"` in your `package.json`.
5.  Add the following code to the file (`index.js`):

    ```js
    import axios from 'axios'
    let data = JSON.stringify({
      jsonrpc: "2.0",
      method: "eth_blockNumber",
      params: [],
      id: "getblock.io",
    });

    let config = {
      method: "post",
      maxBodyLength: Infinity,
      url: "https://go.getblock.us/<ACCESS_TOKEN>",
      headers: {
        "Content-Type": "application/json",
      },
      data: data,
    };

    axios
      .request(config)
      .then((response) => {
        console.log(JSON.stringify(response.data));
      })
      .catch((error) => {
        console.log(error);
      });

    ```

    > Replace `<ACCESS_TOKEN>` with your actual access token from GetBlock.
6.  Run the script:

    ```bash
    node index.js
    ```

    The sequence number and authentication key log in your console like this:

    ```json
    {
        "jsonrpc": "2.0",
        "id": "getblock.io",
        "result": "0x183037af"
    }
    ```

#### Quickstart with Python and Requests

Before you begin, you must have installed Python and Pip on your local machine.

1.  Set up your project using this command:

    ```bash
    mkdir arbitrum-api-quickstart
    cd arbitrum-api-quickstart
    ```
2.  Set up a virtual environment to isolate dependencies:

    ```bash
    python -m venv venv
    source venv/bin/activate  
    # On Windows, use venv\Scripts\activate
    ```
3.  Install the requests library:

    ```bash
    pip install requests
    ```
4.  Create a new file called [`main.py`](http://main.py) and insert the following code:

    ```python
    import requests
    import json

    url = "https://go.getblock.us/<ACCESS_TOKEN>"

    payload = json.dumps({
      "jsonrpc": "2.0",
      "method": "eth_blockNumber",
      "params": [],
      "id": "getblock.io"
    })
    headers = {
      'Content-Type': 'application/json'
    }

    response = requests.request("POST", url, headers=headers, data=payload)

    print(response.text)

    ```

    > Replace `<ACCESS_TOKEN>` with your actual access token from GetBlock.
5.  Run the script:

    ```bash
    python main.py
    ```

