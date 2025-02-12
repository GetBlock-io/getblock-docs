---
description: >-
  This guide explains how GetBlock users can connect to blockchain nodes to
  create accounts and send transactions.
---

# How to generate accounts and send transactions

In blockchains, ‘account’ should be referred to as a pair of private and public keys. Blockchains ‘recognize’ their users and balances by these keypairs.

Unlike login/password pairs in traditional computational solutions, in modern blockchains, every account can be restored with a private key only.

So, to broadcast transactions to a decentralized network, we need first to create (or restore) our account. The whole set of interactions is organized via Web3.js library.

### Creating accounts

First, initialize the Web3.js library and set up the connection to a blockchain node:

```javascript
const Web3 = require('web3');

//Set up the provider (replace ACCESS-TOKEN with your actual token)
const web3 = new Web3('https://go.getblock.io/<ACCESS-TOKEN>/');
```

Next, we can create an account on the [BNB Smart Chain](https://getblock.io/nodes/bsc/) testnet:

```javascript
// Generate new address and private key
const accountTo = web3.eth.accounts.create();
console.log('Generated account:', accountTo);
```

We can also restore an account from an existing private key:

```javascript
// Restore account from private key
const privateKey = process.env['privateKey'];
const accountFrom = web3.eth.accounts.privateKeyToAccount(privateKey);
```

You may ask what does ‘eth’ mean when we’re interacting with BNB Chain? No mistake, it reflects the fact that BNB Smart Chain is fully compatible with Ethereum Virtual Machine.

### Sending transactions

In blockchains, transactions should be signed (authorized) to be ‘included’ into blockchains (confirmed by its consensus of miners or validators).

Here’s how our transactions are created. 0.01 ETH is used for demo.

```javascript
const createSignedTx = async (rawTx) => {
rawTx.gas = await web3.eth.estimateGas(rawTx);
return await accountFrom.signTransaction(rawTx);
}

const sendSignedTx = async (signedTx) => {
// You can use signedTx.rawTransaction as params for
// calling eth_sendRawTransaction JSON-RPC method
web3.eth.sendSignedTransaction(signedTx.rawTransaction).then(
console.log
);
}

const amountTo = "0.01" // ether
```

That’s how the transaction looks before being included in the blockchain. Once signed, it can be sent to the network of nodes.

```javascript
const rawTx = {
to: accountTo.address,
value: web3.utils.toWei(amountTo, 'ether'),
chainId: chainId
};

createSignedTx(rawTx).then(sendSignedTx)
```

That’s it: your account is good to go and its transaction is submitted successfully!
