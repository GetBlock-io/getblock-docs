---
description: >-
  GetBlock’s configuration file provides a more organized and flexible way to
  interact with blockchain nodes and networks without exposing sensitive API
  keys or credentials in the code.
---

# Using GetBlock configuration files

## How to make HTTP requests with curl using JSON config file

Using GetBlock’s JSON configuration file with curl is particularly helpful when you need to access various node endpoints without hardcoding API keys in the code:

1. Download the `getblock.config.json` file from your GetBlock account;
2. Make sure you have [jq](https://jqlang.github.io/jq/download/) installed. jq is a versatile command-line tool that enables extracting values from JSON files;
3. Navigate to your workspace or directory where you have imported the `getblock.config.json` file and open a terminal;
4. Now, you can make a GET request to a selected node endpoint using the curl command:

```bash
curl -X GET https://go.getblock.io/"$(jq -r '.shared.btc.mainnet.rest[0]' getblock.config.json)"/rest/chaininfo.json
```

## How to use GetBlock’s JavaScript config with Web3.js

Connect to Ethereum nodes and other EVM-compatible networks using web3.js and GetBlock’s JS configuration file.

1. Make sure the web3.js library is added to your project. In order to do that, use one of the following methods:

* Npm: `npm install web3`
* Yarn: `yarn add web3`
* Pure js link: `dist/web3.min.js`

2. Download the `getblock.config.js` file from your GetBlock account. Add this file to your project directory.
3. Import the `getblock` module to a .js file that configures a new Web3 instance:

```javascript
const { getblock } = require('./getblock.config.js');
```

4. Connect to an Ethereum node and start sending API calls using web3.js over HTTP or WebSocket in the format below:

```javascript
const { getblock } = require('./getblock.config.js');
var Web3 = require('web3');

// Create the JSON-RPC provider
var web3Rpc = new Web3(new Web3.providers.HttpProvider(
        getblock.shared.eth.mainnet.rpc[0].go()
));

// Create the WebSocket provider
var web3Ws = new Web3.providers.WebsocketProvider(
        `wss://go.getblock.io/${getblock.shared.eth.mainnet.ws[0].token()}`
));
```

Use `go()` method to access an entire endpoint or `token()` to fetch the token.

## How to use the JS config with Hardhat

Set up GetBlock’s JS config file in Hardhat following the steps below:

1. Ensure you have Hardhat installed as a dependency in your Node.js project or run the following command to do so:

```bash
npm install --save-dev hardhat
```

2. Navigate to your GetBlock account and install the `getblock.config.js` file. Copy and paste it into your working directory;
3. Open the `hardhat.config.js` file from your project directory and import the `getblock` module:

```javascript
const { getblock } = require('./getblock.config.js');
```

4. To set up GetBlock as a provider, modify the Hardhat configuration file with the credentials as shown below. Use `go()` method to access an entire endpoint or `token()` to fetch the token only.

```javascript
const { getblock } = require('./getblock.config.js'); 

module.exports = {
  defaultNetwork: "sepolia",
  networks: {
    hardhat: {
    },
    sepolia: {
      url: getblock.shared.eth.sepolia.rpc[0].go() // https://go.getblock.io/<ACCESS-TOKEN>/
    },
    goerli: {
      url: `https://go.getblock.io/${getblock.shared.eth.goerli.rpc[0].token()}` // <ACCESS-TOKEN>
    },
  },
  solidity: {
    version: "0.8.19",
    settings: {
      optimizer: {
        enabled: true,
        runs: 200
      }
    }
  },
  paths: {
    sources: "./contracts",
    tests: "./test",
    cache: "./cache",
    artifacts: "./artifacts"
  },
  mocha: {
    timeout: 40000
  }
}
```
