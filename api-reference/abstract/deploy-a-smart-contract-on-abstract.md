---
description: >-
  Learn how to deploy a smart contract on Abstract using GetBlock's RPC endpoint
  with Hardhat or Foundry.
---

# Deploy a smart contract on Abstract

Abstract is EVM-compatible, but it runs on the ZK Stack's EraVM rather than the standard EVM, so contracts must be compiled with the zkSync `zksolc` compiler — plain `solc` bytecode from vanilla Foundry or Hardhat will not run. In practice this means using the zkSync-aware toolchain (`hardhat-zksync` or `foundry-zksync`). This guide covers adding the network to a wallet and deploying a first contract to Abstract.

## Prerequisites

* [Node.js](https://nodejs.org/) 18+ and a package manager, or [foundry-zksync](https://docs.zksync.io/build/tooling/foundry)
* An EVM wallet (such as MetaMask) with ETH on Abstract for gas — see [Add Network to Your Wallet](deploy-a-smart-contract-on-abstract.md#add-network-to-your-wallet) and [Funding Your Deployer](deploy-a-smart-contract-on-abstract.md#funding-your-deployer)
* A GetBlock access token — the endpoint is `https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/`
* A funded deployer address with ETH on Abstract

## Network Details

| Property        | Value                                                     |
| --------------- | --------------------------------------------------------- |
| Network Name    | Abstract                                                  |
| RPC URL         | `https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/` |
| Chain ID        | 2741 (0xab5)                                              |
| Currency Symbol | ETH                                                       |
| Block Explorer  | [abscan.org](https://abscan.org/)                         |

## Add Network to Your Wallet

Ethereum Mainnet is pre-configured in MetaMask by default. Abstract is not, so it must be added manually.

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

Enter the values from the [Network Details](deploy-a-smart-contract-on-abstract.md#network-details) table above.
{% endstep %}

{% step %}
### Confirm the Chain ID

Confirm the Chain ID resolves to `2741`.
{% endstep %}

{% step %}
### Save and switch networks

Save and switch to the newly added Abstract network.
{% endstep %}
{% endstepper %}

## Funding Your Deployer

Abstract gas is paid in ETH bridged from Ethereum.

* **Mainnet** — bridge ETH from Ethereum L1 using the official Abstract bridge (portal), then send ETH to your deployer's address.
* **Testnet (chain ID 11124)** — obtain Ethereum Sepolia ETH from a public faucet and bridge it to the Abstract testnet, or use an Abstract testnet faucet; see [docs.abs.xyz](https://docs.abs.xyz/) and the `https://api.testnet.abs.xyz` endpoint.

## How to Deploy Smart contract

{% tabs %}
{% tab title="Deploy with Hardhat (zksync)" %}
{% stepper %}
{% step %}
### Create the project and install the zksync plugins

```bash
mkdir hello-abstract && cd hello-abstract
npm init --yes
npm install --save-dev hardhat @matterlabs/hardhat-zksync zksync-ethers ethers
```
{% endstep %}

{% step %}
### Configure the network for the ZK Stack

{% code title="hardhat.config.js" %}
```javascript
require('@matterlabs/hardhat-zksync');

const ABSTRACT_CHAIN_ID = 2741;

module.exports = {
  zksolc: { version: 'latest', settings: {} },
  networks: {
    abstract: {
      url: process.env.ABSTRACT_RPC_URL,
      ethNetwork: 'mainnet',
      zksync: true,
      chainId: ABSTRACT_CHAIN_ID,
      verifyURL: 'https://api.abscan.org/api'
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
    string public greeting = "Hello, Abstract";

    function setGreeting(string calldata greeting_) external {
        greeting = greeting_;
    }
}
```
{% endcode %}
{% endstep %}

{% step %}
### Compile with zksolc and deploy

```bash
export ABSTRACT_RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
export WALLET_PRIVATE_KEY=0xyour_deployer_private_key

npx hardhat compile
npx hardhat deploy-zksync --network abstract
```
{% endstep %}
{% endstepper %}
{% endtab %}

{% tab title="Deploy with foundry-zksync" %}
{% stepper %}
{% step %}
### Install foundry-zksync

```bash
curl -L https://raw.githubusercontent.com/matter-labs/foundry-zksync/main/install-foundry-zksync | bash
foundryup-zksync
forge init hello-abstract && cd hello-abstract
```
{% endstep %}

{% step %}
### Deploy with the --zksync flag

```bash
export ABSTRACT_RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

forge create src/Hello.sol:Hello \
  --zksync \
  --rpc-url $ABSTRACT_RPC_URL \
  --private-key $PRIVATE_KEY \
  --broadcast
```
{% endstep %}

{% step %}
### Verify on the block explorer

Abstract uses Abscan (Etherscan-compatible):

```bash
forge verify-contract <DEPLOYED_ADDRESS> src/Hello.sol:Hello \
  --zksync \
  --chain-id 2741 \
  --verifier etherscan \
  --verifier-url https://api.abscan.org/api \
  --etherscan-api-key <ABSCAN_API_KEY>
```
{% endstep %}
{% endstepper %}
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Because Abstract has native account abstraction, every account is a smart account. Contracts and dApps can integrate the Abstract Global Wallet (AGW) and paymasters to sponsor gas and offer social logins — behaviour that has no equivalent on a standard EVM chain. See the Abstract documentation for AGW and paymaster integration.
{% endhint %}
