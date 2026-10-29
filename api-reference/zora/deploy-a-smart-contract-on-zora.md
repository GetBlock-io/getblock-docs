---
description: Learn how to deploy a smart contract on Zora using GetBlock's RPC endpoint.
---

# Deploy a Smart Contract on Zora

Zora is EVM-compatible, so contracts deploy with the standard Solidity toolchain — Foundry, Hardhat, or Remix — pointed at a GetBlock endpoint, with no chain-specific compiler or plugin required. Gas is paid in ETH. This guide covers adding the network to a wallet and deploying a first contract.

## Prerequisites

* [Node.js](https://nodejs.org/) 18+ and a package manager, or [Foundry](https://book.getfoundry.sh/)
* An EVM wallet (such as MetaMask) with ETH for gas — see [Add Network to Your Wallet](deploy-a-smart-contract-on-zora.md#add-network-to-your-wallet) and [Funding Your Deployer](deploy-a-smart-contract-on-zora.md#funding-your-deployer)
* A GetBlock access token — the endpoint is `https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/`
* A funded deployer address with ETH

## Network Details

| Property        | Value                                                 |
| --------------- | ----------------------------------------------------- |
| Network Name    | Zora Mainnet                                          |
| RPC URL         | https://shared.eu-central-1.getblock.io//             |
| Chain ID        | 7777777 (0x76adf1)                                    |
| Currency Symbol | ETH                                                   |
| Block Explorer  | [explorer.zora.energy](https://explorer.zora.energy/) |

## Add Network to Your Wallet

Zora is not pre-configured in MetaMask by default, so it must be added manually — follow the steps below:

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

Enter the values from the [Network Details](deploy-a-smart-contract-on-zora.md#network-details) table above.
{% endstep %}

{% step %}
### Confirm the Chain ID

Confirm the Chain ID resolves to `7777777`.
{% endstep %}

{% step %}
### Save and switch networks

Save and switch to the newly added Zora network.
{% endstep %}
{% endstepper %}

## Funding Your Deployer

Zora gas is paid in ETH, bridged from Ethereum L1.

* **Mainnet** — bridge ETH to Zora through the official Zora bridge, or withdraw to a Zora address from a supported exchange.
* **Testnet (Zora Sepolia, chain ID 999999999)** — claim test ETH from a Sepolia faucet and bridge to Zora Sepolia; see [Zora Documentation](https://docs.zora.co/) for the current bridge and endpoints.

## Deploy with Foundry

{% stepper %}
{% step %}
### Install Foundry and initialize

```bash
curl -L https://foundry.paradigm.xyz | bash
foundryup
forge init hello-zora && cd hello-zora
```
{% endstep %}

{% step %}
### Write the contract

{% code title="src/Hello.sol" %}
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

contract Hello {
    string public greeting = "Hello, Zora";

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
export RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

forge create src/Hello.sol:Hello \
  --rpc-url $RPC_URL \
  --private-key $PRIVATE_KEY \
  --broadcast
```
{% endstep %}

{% step %}
### Verify on the block explorer

Zora's explorer is Blockscout-based, so verification uses the Blockscout verifier:

```bash
forge verify-contract <DEPLOYED_ADDRESS> src/Hello.sol:Hello \
  --chain-id 7777777 \
  --verifier blockscout \
  --verifier-url https://explorer.zora.energy/api
```
{% endstep %}
{% endstepper %}

## Deploy with Hardhat

{% stepper %}
{% step %}
### Create the project

```bash
mkdir hello-zora && cd hello-zora
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

const CHAIN_ID = 7777777;

module.exports = {
  solidity: '0.8.24',
  networks: {
    zora: {
      url: process.env.RPC_URL,
      chainId: CHAIN_ID,
      accounts: [process.env.PRIVATE_KEY]
    }
  },
  etherscan: {
    apiKey: { zora: 'empty' },
    customChains: [
      {
        network: 'zora',
        chainId: CHAIN_ID,
        urls: {
          apiURL: 'https://explorer.zora.energy/api',
          browserURL: 'https://explorer.zora.energy'
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
    string public greeting = "Hello, Zora";

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
export RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

npx hardhat ignition deploy ./ignition/modules/Hello.js --network zora
```
{% endstep %}

{% step %}
### Verify on the block explorer

```bash
npx hardhat verify --network zora <DEPLOYED_ADDRESS>
```
{% endstep %}
{% endstepper %}

## After Deploying

* Read contract state through the `eth_call` method against your GetBlock endpoint
* Watch contract events with `eth_getLogs`
* Explore your contract and transactions on the [Zora explorer](https://explorer.zora.energy/)
