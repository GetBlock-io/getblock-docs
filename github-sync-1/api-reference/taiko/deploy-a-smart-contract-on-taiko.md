---
description: Learn how to deploy a smart contract on Taiko using GetBlock's RPC endpoint.
---

# Deploy a Smart Contract on Taiko

Taiko is EVM-compatible, so contracts deploy with the standard Solidity toolchain — Foundry, Hardhat, or Remix — pointed at a GetBlock endpoint, with no chain-specific compiler or plugin required. Gas is paid in ETH. This guide covers adding the network to a wallet and deploying a first contract.

## Prerequisites

* [Node.js](https://nodejs.org/) 18+ and a package manager, or [Foundry](https://book.getfoundry.sh/)
* An EVM wallet (such as MetaMask) with ETH for gas — see [Add Network to Your Wallet](deploy-a-smart-contract-on-taiko.md#add-network-to-your-wallet) and [Funding Your Deployer](deploy-a-smart-contract-on-taiko.md#funding-your-deployer)
* A GetBlock access token — the endpoint is `https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/`
* A funded deployer address with ETH

## Network Details

| Property        | Value                                     |
| --------------- | ----------------------------------------- |
| Network Name    | Taiko Mainnet                             |
| RPC URL         | https://shared.eu-central-1.getblock.io// |
| Chain ID        | 167000 (0x28c58)                          |
| Currency Symbol | ETH                                       |
| Block Explorer  | [taikoscan.io](https://taikoscan.io/)     |

## Add Network to Your Wallet

Taiko is not pre-configured in MetaMask by default, so it must be added manually — follow the steps below:

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

Enter the values from the [Network Details](deploy-a-smart-contract-on-taiko.md#network-details) table above.
{% endstep %}

{% step %}
### Confirm the Chain ID

Confirm the Chain ID resolves to `167000`.
{% endstep %}

{% step %}
### Save and switch networks

Save and switch to the newly added Taiko network.
{% endstep %}
{% endstepper %}

## Funding Your Deployer

Taiko gas is paid in ETH, bridged from Ethereum L1.

* **Mainnet** — bridge ETH to Taiko through the official Taiko bridge, or withdraw to a Taiko address from an exchange that supports it.
* **Testnet (Hekla, chain ID 167009)** — claim test ETH from a Sepolia faucet and bridge to Hekla; see [docs.taiko.xyz](https://docs.taiko.xyz/) for the current bridge and endpoints.

## Deploy with Foundry

{% stepper %}
{% step %}
### Install Foundry and initialize

```bash
curl -L https://foundry.paradigm.xyz | bash
foundryup
forge init hello-taiko && cd hello-taiko
```
{% endstep %}

{% step %}
### Write the contract

{% code title="src/Hello.sol" %}
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

contract Hello {
    string public greeting = "Hello, Taiko";

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

Taiko's explorer is Etherscan-family, so verification uses the Etherscan verifier with an API key (set it as an environment variable):

```bash
forge verify-contract <DEPLOYED_ADDRESS> src/Hello.sol:Hello \
  --chain-id 167000 \
  --verifier etherscan \
  --verifier-url https://api.taikoscan.io/api \
  --etherscan-api-key $TAIKOSCAN_API_KEY
```
{% endstep %}
{% endstepper %}

## Deploy with Hardhat

{% stepper %}
{% step %}
### Create the project

```bash
mkdir hello-taiko && cd hello-taiko
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

const CHAIN_ID = 167000;

module.exports = {
  solidity: '0.8.24',
  networks: {
    taiko: {
      url: process.env.RPC_URL,
      chainId: CHAIN_ID,
      accounts: [process.env.PRIVATE_KEY]
    }
  },
  etherscan: {
    apiKey: { taiko: process.env.TAIKOSCAN_API_KEY },
    customChains: [
      {
        network: 'taiko',
        chainId: CHAIN_ID,
        urls: {
          apiURL: 'https://api.taikoscan.io/api',
          browserURL: 'https://taikoscan.io'
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
    string public greeting = "Hello, Taiko";

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

npx hardhat ignition deploy ./ignition/modules/Hello.js --network taiko
```
{% endstep %}

{% step %}
### Verify on the block explorer

```bash
npx hardhat verify --network taiko <DEPLOYED_ADDRESS>
```
{% endstep %}
{% endstepper %}

## After Deploying

* Read contract state through the `eth_call` method against your GetBlock endpoint
* Watch contract events with `eth_getLogs`
* Explore your contract and transactions on the [Taiko explorer](https://taikoscan.io/)
