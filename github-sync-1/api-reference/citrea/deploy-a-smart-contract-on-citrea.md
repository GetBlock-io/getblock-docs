---
description: Learn how to deploy a smart contract on Citrea using GetBlock's JSON-RPC API.
---

# Deploy a Smart Contract on Citrea

Citrea is EVM-compatible, so contracts deploy with the standard Solidity toolchain — Foundry, Hardhat, or Remix — pointed at a GetBlock endpoint, with no chain-specific compiler or plugin required. Gas is paid in cBTC. This guide covers adding the network to a wallet and deploying a first contract.

## Prerequisites

* [Node.js](https://nodejs.org/) 18+ and a package manager, or [Foundry](https://book.getfoundry.sh/)
* An EVM wallet (such as MetaMask) with cBTC for gas — see [Add Network to Your Wallet](deploy-a-smart-contract-on-citrea.md#add-network-to-your-wallet)
* A GetBlock access token — the endpoint is `https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/`
* A funded deployer address with cBTC

## Network Details

| Property        | Value                                                               |
| --------------- | ------------------------------------------------------------------- |
| Network Name    | Citrea                                                              |
| RPC URL         | https://shared.us-east-1.getblock.io//                              |
| Chain ID        | 4114                                                                |
| Currency Symbol | cBTC                                                                |
| Block Explorer  | [explorer.mainnet.citrea.xyz](https://explorer.mainnet.citrea.xyz/) |

## Add Network to Your Wallet

Citrea is not pre-configured in MetaMask by default, so it must be added manually — follow the steps below:

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

Enter the values from the [Network Details](deploy-a-smart-contract-on-citrea.md#network-details) table above.
{% endstep %}

{% step %}
### Confirm the Chain ID

Confirm the Chain ID resolves to `5115`.
{% endstep %}

{% step %}
### Save and switch networks

Save and switch to the newly added Citrea network.
{% endstep %}
{% endstepper %}

## Deploy with Foundry

{% stepper %}
{% step %}
### Install Foundry and initialize

```bash
curl -L https://foundry.paradigm.xyz | bash
foundryup
forge init hello-citrea && cd hello-citrea
```
{% endstep %}

{% step %}
### Write the contract

{% code title="src/Hello.sol" %}
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

contract Hello {
    string public greeting = "Hello, Citrea";

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

Citrea's explorer is Blockscout-based, so verification uses the Blockscout verifier:

```bash
forge verify-contract <DEPLOYED_ADDRESS> src/Hello.sol:Hello \
  --chain-id 5115 \
  --verifier blockscout \
  --verifier-url https://explorer.mainnet.citrea.xyz/api/
```
{% endstep %}
{% endstepper %}

## Deploy with Hardhat

{% stepper %}
{% step %}
### Create the project

```bash
mkdir hello-citrea && cd hello-citrea
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

const CHAIN_ID = 5115;

module.exports = {
  solidity: '0.8.24',
  networks: {
    citrea: {
      url: process.env.RPC_URL,
      chainId: CHAIN_ID,
      accounts: [process.env.PRIVATE_KEY]
    }
  },
  etherscan: {
    apiKey: { citrea: 'empty' },
    customChains: [
      {
        network: 'citrea',
        chainId: CHAIN_ID,
        urls: {
          apiURL: 'https://explorer.mainnet.citrea.xyz/api',
          browserURL: 'https://explorer.mainnet.citrea.xyz'
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
    string public greeting = "Hello, Citrea";

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

npx hardhat ignition deploy ./ignition/modules/Hello.js --network citrea
```
{% endstep %}

{% step %}
### Verify on the block explorer

```bash
npx hardhat verify --network citrea <DEPLOYED_ADDRESS>
```
{% endstep %}
{% endstepper %}

## After Deploying

* Read contract state through the `eth_call` method against your GetBlock endpoint.
* Watch contract events with `eth_getLogs`.
* Explore your contract and transactions on the [Citrea explorer](https://explorer.mainnet.citrea.xyz/).
