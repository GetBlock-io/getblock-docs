---
description: Learn how to deploy a smart contract on 0G using GetBlock's RPC endpoint.
---

# Deploy a smart contract on 0G

0G is EVM-compatible, so contracts deploy with the standard Solidity toolchain — Foundry, Hardhat, or Remix — pointed at a GetBlock endpoint. This guide covers adding the network to a wallet and deploying a first contract to 0G.

## Prerequisites

* [Node.js](https://nodejs.org/) 18+ and a package manager, or [Foundry](https://book.getfoundry.sh/)
* An EVM wallet (such as MetaMask) with 0G on the network for gas — see [Add Network to Your Wallet](deploy-a-smart-contract-on-0g.md#add-network-to-your-wallet) and [Funding Your Deployer](deploy-a-smart-contract-on-0g.md#funding-your-deployer)
* A GetBlock access token — the endpoint is `https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/`
* A funded deployer address with 0G

## Network Details

| Property        | Value                                       |
| --------------- | ------------------------------------------- |
| Network Name    | 0G                                          |
| RPC URL         | `https://shared.eu-central-1.getblock.io/`  |
| Chain ID        | 16661 (0x4115)                              |
| Currency Symbol | 0G                                          |
| Block Explorer  | [chainscan.0g.ai](https://chainscan.0g.ai/) |

## Add Network to Your Wallet

Ethereum Mainnet is pre-configured in MetaMask by default. 0G is not, so it must be added manually

{% stepper %}
{% step %}
#### Open your wallet

Open MetaMask (or your EVM wallet of choice).
{% endstep %}

{% step %}
#### Add a custom network

Open the network selector and choose **Add a custom network**.
{% endstep %}

{% step %}
#### Enter the network details

Enter the values from the [Network Details](deploy-a-smart-contract-on-0g.md#network-details) table above.
{% endstep %}

{% step %}
#### Confirm the Chain ID

Confirm the Chain ID resolves to `16661`.
{% endstep %}

{% step %}
#### Save and switch networks

Save and switch to the newly added 0G network.
{% endstep %}
{% endstepper %}

## Funding Your Deployer

0G gas is paid in the native 0G token.

* **Mainnet** — acquire 0G from an exchange or bridge, then send it to your deployer's address.
* **Testnet (Galileo, chain ID 16602)** — request test 0G from the 0G Galileo faucet; see [docs.0g.ai](https://docs.0g.ai/) for the current faucet and the `https://evmrpc-testnet.0g.ai` endpoint.

## How to Deploy Smart Contract

{% tabs %}
{% tab title="Deploy with Foundry" %}
{% stepper %}
{% step %}
### Install Foundry and initialize

```bash
curl -L https://foundry.paradigm.xyz | bash
foundryup
forge init hello-0g && cd hello-0g
```
{% endstep %}

{% step %}
### Write the contract

{% code title="src/Hello.sol" %}
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

contract Hello {
    string public greeting = "Hello, 0G";

    function setGreeting(string calldata greeting_) external {
        greeting = greeting_;
    }
}
```
{% endcode %}
{% endstep %}

{% step %}
### Deploy

0G targets the Cancun EVM; compile for that target when you deploy:

```bash
export ZEROG_RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

forge create src/Hello.sol:Hello \
  --rpc-url $ZEROG_RPC_URL \
  --private-key $PRIVATE_KEY \
  --evm-version cancun \
  --broadcast
```
{% endstep %}

{% step %}
### Verify the source

0G Chain Scan supports source verification through Sourcify:

```bash
forge verify-contract <DEPLOYED_ADDRESS> src/Hello.sol:Hello \
  --chain-id 16661 \
  --verifier sourcify
```
{% endstep %}
{% endstepper %}
{% endtab %}

{% tab title="Deploy with Hardhat" %}
{% stepper %}
{% step %}
### Create the project

```bash
mkdir hello-0g && cd hello-0g
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

const ZEROG_CHAIN_ID = 16661;

module.exports = {
  solidity: {
    version: '0.8.24',
    settings: { evmVersion: 'cancun' }
  },
  networks: {
    zerog: {
      url: process.env.ZEROG_RPC_URL,
      chainId: ZEROG_CHAIN_ID,
      accounts: [process.env.PRIVATE_KEY]
    }
  },
  sourcify: {
    enabled: true
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
    string public greeting = "Hello, 0G";

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
export ZEROG_RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

npx hardhat ignition deploy ./ignition/modules/Hello.js --network zerog
```
{% endstep %}

{% step %}
### Verify the source

```bash
npx hardhat verify --network zerog <DEPLOYED_ADDRESS>
```
{% endstep %}
{% endstepper %}
{% endtab %}
{% endtabs %}

{% hint style="info" %}
0G's EVM targets the Cancun opcode set. If your compiler defaults to a newer EVM version, set `evm_version = "cancun"` (Foundry) or `evmVersion: 'cancun'` (Hardhat), and consider an established Solidity version (for example 0.8.24) to avoid unsupported opcodes.
{% endhint %}
