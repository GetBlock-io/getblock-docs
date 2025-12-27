---
description: >-
  GetBlock provides fast and reliable access to Dogecoin nodes via JSON-RPC API.
  Connect to the Dogecoin network without running your own infrastructure.
---

# Dogecoin (DOGE)

### Overview

Dogecoin is a decentralized, peer-to-peer cryptocurrency that was created in December 2013 as a lighthearted alternative to Bitcoin. Originally started as a joke based on the popular "Doge" Shiba Inu meme, Dogecoin has grown into a legitimate digital currency with a vibrant, philanthropic community.

### Key Features

* **Fast Transactions**: 1-minute block time (vs Bitcoin's 10 minutes)
* **Low Fees**: Transaction fees are significantly lower than Bitcoin
* **Inflationary Supply**: No maximum supply cap, with \~5 billion new DOGE mined annually
* **Scrypt Algorithm**: Uses Scrypt proof-of-work, merged mining with Litecoin
* **Active Community**: Large, engaged community focused on tipping and charitable giving
* **Wide Adoption**: Accepted by many merchants and supported by major exchanges

## Network Information

| Property        | Value                     |
| --------------- | ------------------------- |
| Network Name    | Dogecoin Mainnet          |
| Currency Symbol | DOGE                      |
| Block Time      | \~1 minute                |
| Consensus       | Proof of Work (Scrypt)    |
| RPC Port        | 22555                     |
| P2P Port        | 22556                     |
| Address Prefix  | D (mainnet)               |
| Smallest Unit   | 1 Koinu = 0.00000001 DOGE |

## Quickstart

In this section, you will learn how to make your first call with either:

* Axios
* Python

### Quickstart with Axios

Before you begin, you must have already installed `npm` or `yarn` on your local machine. If not, check out [npm](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm) or [yarn](https://classic.yarnpkg.com/lang/en/docs/install).

1. Set up your project using this command:

{% tabs %}
{% tab title="npm" %}
```bash
mkdir dogecoin-api-quickstart
cd dogecoin-api-quickstart
npm init --yes
```
{% endtab %}

{% tab title="Yarn" %}
```bash
mkdir doegcoin-api-quickstart
cd dogecoin-api-quickstart
yarn init --yes
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
This creates a project directory named `dogecoin-api-quickstart` and initialises a Node.js project within it.
{% endhint %}

2. Install Axios using this command:\
   Using npm:

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
        "jsonrpc": "2.0",
        "method": "getinfo",
        "params": [],
        "id": "getblock.io"
    });

    let config = {
      method: "post",
      maxBodyLength: Infinity,
      url: "https://go.getblock.io/<ACCESS_TOKEN>",
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
        "result": {
            "version": 1140900,
            "protocolversion": 70015,
            "blocks": 6010614,
            "timeoffset": 0,
            "connections": 8,
            "proxy": "",
            "difficulty": 45990648.91368412,
            "testnet": false,
            "paytxfee": 0.01000000,
            "relayfee": 0.00100000,
            "errors": ""
        },
        "error": null,
        "id": "getblock.io"
    }
    ```

### Quickstart with Python and Requests

Before you begin, you must have installed Python and Pip on your local machine.

1.  Set up your project using this command:

    ```bash
    mkdir dogecoin-api-quickstart
    cd dogecoin-api-quickstart
    ```
2.  Set up a virtual environment to isolate dependencies:

    ```bash
    python -m venv venv

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

    url = "https://go.getblock.io/<ACCESS_TOKEN>"

    payload = json.dumps({
        "jsonrpc": "2.0",
        "method": "getinfo",
        "params": [],
        "id": "getblock.io"
    })

    headers = {
        'Content-Type': 'application/json'
    }

    response = requests.post(url, headers=headers, data=payload)
    print(response.text)
    ```



{% hint style="info" %}
Replace `<ACCESS_TOKEN>` with your actual access token from GetBlock.
{% endhint %}

5. Run the script:

```bash
python main.py
```

### Base URL

{% tabs %}
{% tab title="Franfurt, Germany" %}
```bash
https://go.getblock.io
```
{% endtab %}
{% endtabs %}

### Supported Network

* Mainnet

### Available API Interface

* JSON RPC
* Blockbook(WS)
* REST
* Blockbook(REST)

## Available API Methods

GetBlock provides access to standard Dogecoin Core JSON-RPC methods.

| Method               | Description                             |
| -------------------- | --------------------------------------- |
| createrawtransaction | Creates a raw transaction               |
| decoderawtransaction | Decodes a raw transaction               |
| getblock             | Returns block data for a given hash     |
| getblockcount        | Returns the current block height        |
| getblockhash         | Returns block hash at given height      |
| getconnectioncount   | Returns number of peer connections      |
| getdifficulty        | Returns the current mining difficulty   |
| gethashespersec      | Returns hash rate per second            |
| getinfo              | Returns general node information        |
| getmininginfo        | Returns mining-related information      |
| getrawtransaction    | Returns raw transaction data            |
| gettransaction       | Returns detailed transaction info       |
| gettxout             | Returns details about an unspent output |
| signmessage          | Signs a message with a private key      |
| signrawtransaction   | Signs a raw transaction                 |
| validateaddress      | Validates a Dogecoin address            |
| verifymessage        | Verifies a signed message               |

## Support

For technical support and questions:

* Support: [support@getblock.io](mailto:support@getblock.io)

## See Also

* [Dogecoin Official Documentation](https://dogecoin.com/)
* [Dogecoin GitHub Repository](https://github.com/dogecoin/dogecoin)
* [Dogecoin Block Explorer](https://dogechain.info/)
