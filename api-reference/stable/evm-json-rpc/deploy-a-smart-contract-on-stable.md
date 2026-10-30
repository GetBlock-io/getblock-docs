---
description: >-
  Complete guide on how to deploy a smart contract on Stable using GetBlock Web3
  documentation.
---

# Deploy a Smart Contract on Stable

Stable is EVM-compatible, so contracts deploy with the standard Solidity toolchain — Foundry, Hardhat, or Remix — pointed at a GetBlock endpoint, with no chain-specific compiler or plugin required. Gas is paid in USDT0. This guide covers adding the network to a wallet and deploying a first contract.

## Prerequisites

* [Node.js](https://nodejs.org/) 18+ and a package manager, or [Foundry](https://book.getfoundry.sh/)
* An EVM wallet (such as MetaMask) with USDT0 for gas — see [Add Network to Your Wallet](deploy-a-smart-contract-on-stable.md#add-network-to-your-wallet) and [Funding Your Deployer](deploy-a-smart-contract-on-stable.md#funding-your-deployer)
* A GetBlock access token — the EVM endpoint is `https://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/`
* A funded deployer address with USDT0

## Network Details

| Property        | Value                                                      |
| --------------- | ---------------------------------------------------------- |
| Network Name    | Stable Mainnet                                             |
| RPC URL         | https://shared.ap-southeast-1.getblock.io/\<ACCESS-TOKEN>/ |
| Chain ID        | 988 (0x3dc)                                                |
| Currency Symbol | USDT0                                                      |
| Block Explorer  | [stablescan.xyz](https://stablescan.xyz/)                  |

{% hint style="info" %}
USDT0 is the native gas token with **18 decimals** (wei-style). The USDT0 ERC-20 token contract (`0x779Ded0c9e1022225f8E0630b35a9b54bE713736`) uses **6 decimals** — use 18-decimal math for gas and native balances, and 6-decimal math for ERC-20 transfers.
{% endhint %}

## Add Network to Your Wallet

Stable is not pre-configured in MetaMask by default, so it must be added manually — follow the steps below:

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

Enter the values from the [Network Details](deploy-a-smart-contract-on-stable.md#network-details) table above.
{% endstep %}

{% step %}
### Confirm the Chain ID

Confirm the Chain ID resolves to `988`.
{% endstep %}

{% step %}
### Save and switch networks

Save and switch to the newly added Stable network.
{% endstep %}
{% endstepper %}

## Funding Your Deployer

Stable gas is paid in USDT0.

* **Mainnet** — acquire USDT0 (or bridge USDT) to Stable, then send it to your deployer's address.
* See [Stable documentation](https://docs.stable.xyz/) for the current bridge and faucet endpoints.

## Deploy with Foundry

{% stepper %}
{% step %}
### Install Foundry and initialize

```bash
curl -L https://foundry.paradigm.xyz | bash
foundryup
forge init hello-stable && cd hello-stable
```
{% endstep %}

{% step %}
### Write the contract

{% code title="src/Hello.sol" %}
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

contract Hello {
    string public greeting = "Hello, Stable";

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
export RPC_URL=https://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

forge create src/Hello.sol:Hello \
  --rpc-url $RPC_URL \
  --private-key $PRIVATE_KEY \
  --broadcast
```
{% endstep %}

{% step %}
### Verify on the block explorer

Stable's explorer is Blockscout-based, so verification uses the Blockscout verifier:

```bash
forge verify-contract <DEPLOYED_ADDRESS> src/Hello.sol:Hello \
  --chain-id 988 \
  --verifier blockscout \
  --verifier-url https://stablescan.xyz/api/
```
{% endstep %}
{% endstepper %}

## Deploy with Hardhat

{% stepper %}
{% step %}
### Create the project

```bash
mkdir hello-stable && cd hello-stable
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

const CHAIN_ID = 988;

module.exports = {
  solidity: '0.8.24',
  networks: {
    stable: {
      url: process.env.RPC_URL,
      chainId: CHAIN_ID,
      accounts: [process.env.PRIVATE_KEY]
    }
  },
  etherscan: {
    apiKey: { stable: 'empty' },
    customChains: [
      {
        network: 'stable',
        chainId: CHAIN_ID,
        urls: {
          apiURL: 'https://stablescan.xyz/api',
          browserURL: 'https://stablescan.xyz'
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
    string public greeting = "Hello, Stable";

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
export RPC_URL=https://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

npx hardhat ignition deploy ./ignition/modules/Hello.js --network stable
```
{% endstep %}

{% step %}
### Verify on the block explorer

```bash
npx hardhat verify --network stable <DEPLOYED_ADDRESS>
```
{% endstep %}
{% endstepper %}

## After Deploying

* Read contract state through the `eth_call` method against your GetBlock endpoint.
* Watch contract events with `eth_getLogs`.
* Explore your contract and transactions on [Stablescan](https://stablescan.xyz/).
