---
description: >-
  Set up GetBlock as a provider using Ethers.js library to interact with the
  blockchain and streamline your dApp development process.
---

# Ethers.js integration

Ethers.js is a lightweight JavaScript library for interacting with Ethereum and other EVM-compatible blockchains. It is commonly used by developers to build decentralized applications (dApps) and manage Ethereum-based operations like deploying smart contracts, interacting with them, and managing user wallets.

### Install Ethers.js

Add Ethers.js to your project using your preferred package manager:

* npm

```bash
npm install ethers
```

* yarn

```bash
yarn add ethers
```

### Set GetBlock as a provider

```javascript
// Import the Ethers library
const { ethers } = require('ethers');

// Set up the provider (replace ACCESS_TOKEN with your actual token)
const provider = new ethers.JsonRpcProvider('https://go.getblock.io/ACCESS_TOKEN');

//Call a method using the provider
const main = async () => {
  const blockNumber = await provider.getBlockNumber();
  console.log("Latest Block Number:", blockNumber);
};

// Call the main function
main();
```

For further details and advanced usage, explore the [Ethers.js documentation](https://docs.ethers.org/v6/getting-started/).
