---
description: >-
  Learn how to deploy a smart contract on Cronos zkEVM using GetBlock's RPC
  endpoint.
---

# Deploy a Smart Contract on Cronos zkEVM

Cronos zkEVM is a **ZKsync ZK Stack** chain, so its EVM is compatible at the application level but contracts must be compiled with the ZKsync Solidity compiler (`zksolc`) and deployed with the ZKsync toolchain — standard `solc`/vanilla Foundry bytecode is not deployed directly. Use `@matterlabs/hardhat-zksync` (or `foundry-zksync`). Gas is paid in zkCRO. This guide covers adding the network to a wallet and deploying with Hardhat + ZKsync.

{% hint style="warning" %}
Do not deploy Cronos zkEVM contracts with vanilla `solc`/Foundry as you would on a standard EVM chain. Cronos zkEVM requires the `zksolc` compiler; deploy and verify through `@matterlabs/hardhat-zksync` or `foundry-zksync`.
{% endhint %}

## Prerequisites

* [Node.js](https://nodejs.org/) 18+ and a package manager
* An EVM wallet (such as MetaMask) with zkCRO for gas — see [Add Network to Your Wallet](deploy-a-smart-contract-on-cronos-zkevm.md#add-network-to-your-wallet) and [Funding Your Deployer](deploy-a-smart-contract-on-cronos-zkevm.md#funding-your-deployer)
* A GetBlock access token — the endpoint is `https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/`
* A funded deployer address with zkCRO

## Network Details

| Property        | Value                                                           |
| --------------- | --------------------------------------------------------------- |
| Network Name    | Cronos zkEVM Mainnet                                            |
| RPC URL         | https://shared.eu-central-1.getblock.io//                       |
| Chain ID        | 388 (0x184)                                                     |
| Currency Symbol | zkCRO                                                           |
| Block Explorer  | [explorer.zkevm.cronos.org](https://explorer.zkevm.cronos.org/) |

## Add Network to Your Wallet

Cronos zkEVM is not pre-configured in MetaMask by default, so it must be added manually — follow the steps below:

{% stepper %}
{% step %}
### Open your wallet

Open MetaMask (or your EVM wallet of choice).
{% endstep %}

{% step %}
### Add a custom network

Open the network selector and choose **Add a custom network**.
{% endstep %}

{% step %}
### Enter the network details

Enter the values from the [Network Details](deploy-a-smart-contract-on-cronos-zkevm.md#network-details) table above.
{% endstep %}

{% step %}
### Confirm the chain ID

Confirm the Chain ID resolves to `388`.
{% endstep %}

{% step %}
### Save and switch networks

Save and switch to the newly added Cronos zkEVM network.
{% endstep %}
{% endstepper %}

## Funding Your Deployer

Cronos zkEVM gas is paid in zkCRO.

* **Mainnet** — bridge to Cronos zkEVM through the official bridge, or acquire zkCRO from a supported exchange.

## Deploy with Hardhat + ZKsync

{% stepper %}
{% step %}
### Create the project and install the ZKsync toolchain

```bash
mkdir hello-cronos-zkevm && cd hello-cronos-zkevm
npm init --yes
npm install --save-dev hardhat @matterlabs/hardhat-zksync zksync-ethers ethers
```
{% endstep %}

{% step %}
### Configure zksolc and the network

{% code title="hardhat.config.js" %}
```javascript
require('@matterlabs/hardhat-zksync');

module.exports = {
  zksolc: {
    version: 'latest',
    settings: {}
  },
  defaultNetwork: 'cronosZkevm',
  networks: {
    cronosZkevm: {
      url: process.env.RPC_URL,
      ethNetwork: 'mainnet',
      zksync: true,
      verifyURL: 'https://explorer.zkevm.cronos.org/api'
    }
  },
  solidity: { version: '0.8.24' }
};
```
{% endcode %}
{% endstep %}

{% step %}
### Write the contract

{% code title="contracts/Hello.sol" %}
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

contract Hello {
    string public greeting = "Hello, Cronos zkEVM";

    function setGreeting(string calldata greeting_) external {
        greeting = greeting_;
    }
}
```
{% endcode %}
{% endstep %}

{% step %}
### Compile with zksolc

```bash
npx hardhat compile
```
{% endstep %}

{% step %}
### Deploy

{% code title="deploy/deploy.js" %}
```javascript
const { Deployer } = require('@matterlabs/hardhat-zksync');
const { Wallet } = require('zksync-ethers');

module.exports = async function (hre) {
  const wallet = new Wallet(process.env.PRIVATE_KEY);
  const deployer = new Deployer(hre, wallet);
  const artifact = await deployer.loadArtifact('Hello');
  const contract = await deployer.deploy(artifact, []);
  console.log('Deployed to:', await contract.getAddress());
};
```
{% endcode %}

```bash
export RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

npx hardhat deploy-zksync --script deploy.js
```
{% endstep %}

{% step %}
### Verify on the block explorer

```bash
npx hardhat verify --network cronosZkevm <DEPLOYED_ADDRESS>
```
{% endstep %}
{% endstepper %}

## After Deploying

* Read contract state through the `eth_call` method against your GetBlock endpoint.
* Watch contract events with `eth_getLogs`.
* Explore your contract and transactions on the [Cronos zkEVM explorer](https://explorer.zkevm.cronos.org/).
