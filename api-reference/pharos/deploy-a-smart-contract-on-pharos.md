---
description: Learn how to deploy a smart contract on Pharos using GetBlock's RPC endpoint.
---

# Deploy a smart contract on Pharos

Pharos is EVM-compatible, so contracts deploy with the standard Solidity toolchain — Foundry, Hardhat, or Remix — pointed at a GetBlock endpoint, with no chain-specific compiler or plugin required. Gas is paid in PHRS. This guide covers adding the network to a wallet and deploying a first contract.

## Prerequisites

* [Node.js](https://nodejs.org/) 18+ and a package manager, or [Foundry](https://book.getfoundry.sh/)
* An EVM wallet (such as MetaMask) with PHRS for gas — see [Add Network to Your Wallet](deploy-a-smart-contract-on-pharos.md#add-network-to-your-wallet) and [Funding Your Deployer](deploy-a-smart-contract-on-pharos.md#funding-your-deployer)
* A GetBlock access token — the endpoint is `https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/`
* A funded deployer address with PHRS

## Network Details

| Property        | Value                                                      |
| --------------- | ---------------------------------------------------------- |
| Network Name    | Pharos                                                     |
| RPC URL         | https://shared.us-east-1.getblock.io//                     |
| Chain ID        | 1672(0x688)                                                |
| Currency Symbol | PHRS                                                       |
| Block Explorer  | [https://www.pharosscan.xyz/](https://www.pharosscan.xyz/) |

## Add Network to Your Wallet

Pharos is not pre-configured in MetaMask by default, so it must be added manually — follow the steps below:

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
### Enter network details

Enter the values from the [Network Details](deploy-a-smart-contract-on-pharos.md#network-details) table above.
{% endstep %}

{% step %}
### Confirm the Chain ID

Confirm the Chain ID resolves to `1672`.
{% endstep %}

{% step %}
### Save and switch networks

Save and switch to the newly added Pharos network.
{% endstep %}
{% endstepper %}

## Deploy with Foundry

{% stepper %}
{% step %}
### Install Foundry and initialize

```bash
curl -L https://foundry.paradigm.xyz | bash
foundryup
forge init hello-pharos && cd hello-pharos
```
{% endstep %}

{% step %}
### Write the contract

{% code title="src/Hello.sol" %}
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

contract Hello {
    string public greeting = "Hello, Pharos";

    function setGreeting(string calldata greeting_) external {
        greeting = greeting_;
    }
}
```
{% endcode %}
{% endstep %}

{% step %}
### Deploy

```bash
export RPC_URL=https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

forge create src/Hello.sol:Hello \
  --rpc-url $RPC_URL \
  --private-key $PRIVATE_KEY \
  --broadcast
```
{% endstep %}

{% step %}
### Verify on the block explorer

Pharos's explorer is Blockscout-based, so verification uses the Blockscout verifier:

```bash
forge verify-contract <DEPLOYED_ADDRESS> src/Hello.sol:Hello \
  --chain-id 1672 \
  --verifier blockscout \
  --verifier-url https://atlantic.pharosscan.xyz/api/
```
{% endstep %}
{% endstepper %}

## Deploy with Hardhat

{% stepper %}
{% step %}
### Create the project

```bash
mkdir hello-pharos && cd hello-pharos
npm init --yes
npm install --save-dev hardhat
npx hardhat init
```
{% endstep %}

{% step %}
### Configure the network

{% code title="hardhat.config.js" %}
```javascript
require('@nomicfoundation/hardhat-toolbox');

const CHAIN_ID = 688689;

module.exports = {
  solidity: '0.8.24',
  networks: {
    pharos: {
      url: process.env.RPC_URL,
      chainId: CHAIN_ID,
      accounts: [process.env.PRIVATE_KEY]
    }
  },
  etherscan: {
    apiKey: { pharos: 'empty' },
    customChains: [
      {
        network: 'pharos',
        chainId: CHAIN_ID,
        urls: {
          apiURL: 'https://atlantic.pharosscan.xyz/api',
          browserURL: 'https://atlantic.pharosscan.xyz'
        }
      }
    ]
  }
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
    string public greeting = "Hello, Pharos";

    function setGreeting(string calldata greeting_) external {
        greeting = greeting_;
    }
}
```
{% endcode %}
{% endstep %}

{% step %}
### Deploy

{% code overflow="wrap" %}
```bash
export RPC_URL=https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

npx hardhat ignition deploy ./ignition/modules/Hello.js --network pharos
```
{% endcode %}
{% endstep %}

{% step %}
### Verify on the block explorer

```bash
npx hardhat verify --network pharos <DEPLOYED_ADDRESS>
```
{% endstep %}
{% endstepper %}

## After Deploying

* Read contract state through the `eth_call` method against your GetBlock endpoint
* Watch contract events with `eth_getLogs`
* Explore your contract and transactions on the [Pharos explorer](https://www.pharosscan.xyz/)
