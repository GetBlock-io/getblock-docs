---
description: Learn how to deploy a smart contract on RISE using GetBlock's RPC endpoint.
---

# Deploy a Smart Contract on RISE

RISE  is EVM-compatible, so contracts deploy with the standard Solidity toolchain — Foundry, Hardhat, or Remix — pointed at a GetBlock endpoint, with no chain-specific compiler or plugin required. Gas is paid in ETH. This guide covers adding the network to a wallet and deploying a first contract.

## Prerequisites

* [Node.js](https://nodejs.org/) 18+ and a package manager, or [Foundry](https://book.getfoundry.sh/)
* An EVM wallet (such as MetaMask) with ETH for gas — see [Add Network to Your Wallet](deploy-a-smart-contract-on-rise.md#add-network-to-your-wallet)
* A GetBlock access token — the endpoint is `https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/`
* A funded deployer address with ETH

## Network Details

| Property        | Value                                                              |
| --------------- | ------------------------------------------------------------------ |
| Network Name    | Mainnet                                                            |
| RPC URL         | https://shared.us-east-1.getblock.io//                             |
| Chain ID        | 4153 (0x1039)                                                      |
| Currency Symbol | ETH                                                                |
| Block Explorer  | [https://explorer.risechain.com/](https://explorer.risechain.com/) |

## Add Network to Your Wallet

RISE  is not pre-configured in MetaMask by default, so it must be added manually — follow the steps below:

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

Enter the values from the [Network Details](deploy-a-smart-contract-on-rise.md#network-details) table above.
{% endstep %}

{% step %}
### Confirm the Chain ID

Confirm the Chain ID resolves to `4153`.
{% endstep %}

{% step %}
### Save and switch networks

Save and switch to the newly added RISE  network.
{% endstep %}
{% endstepper %}

## Deploy with Foundry

{% stepper %}
{% step %}
### Install Foundry and initialize

```bash
curl -L https://foundry.paradigm.xyz | bash
foundryup
forge init hello-rise && cd hello-rise
```
{% endstep %}

{% step %}
### Write the contract

{% code title="src/Hello.sol" %}
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

contract Hello {
    string public greeting = "Hello, RISE";

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

RISE Testnet's explorer is Blockscout-based, so verification uses the Blockscout verifier:

```bash
forge verify-contract <DEPLOYED_ADDRESS> src/Hello.sol:Hello \
  --chain-id 4153 \
  --verifier blockscout \
  --verifier-url https://explorer.risechain.com/api/
```
{% endstep %}
{% endstepper %}

## Deploy with Hardhat

{% stepper %}
{% step %}
### Create the project

```bash
mkdir hello-rise && cd hello-rise
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

const CHAIN_ID = 11155931;

module.exports = {
  solidity: '0.8.24',
  networks: {
    rise: {
      url: process.env.RPC_URL,
      chainId: CHAIN_ID,
      accounts: [process.env.PRIVATE_KEY]
    }
  },
  etherscan: {
    apiKey: { rise: 'empty' },
    customChains: [
      {
        network: 'rise',
        chainId: CHAIN_ID,
        urls: {
          apiURL: 'https://explorer.risechain.com/api',
          browserURL: 'https://explorer.risechain.com/
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
    string public greeting = "Hello, RISE";

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

npx hardhat ignition deploy ./ignition/modules/Hello.js --network rise
```
{% endstep %}

{% step %}
### Verify on the block explorer

```bash
npx hardhat verify --network rise <DEPLOYED_ADDRESS>
```
{% endstep %}
{% endstepper %}

## After Deploying

* Read contract state through the `eth_call` method against your GetBlock endpoint
* Watch contract events with&#x20;
* Explore your contract and transactions on the [RISE explorer](https://explorer.risechain.com/)
