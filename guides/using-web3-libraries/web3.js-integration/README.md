---
description: >-
  Learn how to use Web3.js, a widely-used JavaScript library for connecting to
  GetBlock nodes.
---

# Web3.js integration

Web3.js is a JavaScript library built for interacting with the Ethereum blockchain and other EVM-compatible chains. It is used to send JSON-RPC calls to the Ethereum node via HTTP, IPC, or WebSocket connection to read data from the blockchain, make transactions, or deploy smart contracts.

### Install Web3.js

Use your preferred package manager:

{% tabs %}
{% tab title="npm" %}
```bash
npm install web3
```
{% endtab %}

{% tab title="yarn" %}
```bash
yarn add web3
```
{% endtab %}

{% tab title="Pure js link" %}
```bash
dist/web3.min.js
```
{% endtab %}
{% endtabs %}

### Set up your connection to GetBlock

```javascript
// Import the Web3 library
const Web3 = require('web3');

// Set GetBlock as the provider (replace ACCESS_TOKEN with your actual token)
var web3 = new Web3('https://go.getblock.io/ACCESS_TOKEN');

// Initialize web3 method
web3.eth.getBlockNumber().then(console.log);
```

For additional methods and options, refer to the official [Web3.js documentation](https://web3js.readthedocs.io/en/v1.10.0/index.html).
