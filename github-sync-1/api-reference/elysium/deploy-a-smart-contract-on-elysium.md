---
description: Learn how to deploy a smart contract on Elysium using GetBlock's RPC endpoint.
---

# Deploy a Smart Contract on Elysium

Elysium runs Arbitrum Nitro, so it executes standard Ethereum bytecode and contracts deploy with the ordinary Solidity toolchain — Foundry, Hardhat, or Remix — pointed at a GetBlock endpoint, with no chain-specific compiler, plugin, or custom bytecode format. The only differences from an Ethereum deploy are that gas is paid in **HYPE** (bridged 1:1 from HyperEVM) and that, as a Nitro L2, each transaction also carries the cost of posting its data to the parent chain. This guide covers adding Elysium to a wallet and deploying a first contract.

### Prerequisites

* [Node.js](https://nodejs.org/) 18+ and a package manager (npm, pnpm, or yarn)
* A GetBlock access token — create one at [account.getblock.io](https://account.getblock.io/)
* A funded account with HYPE for gas on Elysium
* A development framework — this guide shows [Foundry](https://book.getfoundry.sh/) and [Hardhat](https://hardhat.org/)

### Network Details

| Property        | Value                                                     |
| --------------- | --------------------------------------------------------- |
| Network Name    | Elysium Mainnet                                           |
| RPC URL         | `https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/` |
| Chain ID        | 99901 (0x1863d)                                           |
| Currency Symbol | HYPE                                                      |
| Block Explorer  | See note below                                            |

### Add Elysium to MetaMask

{% stepper %}
{% step %}
#### Open network settings

In MetaMask, open the network dropdown and choose **Add a custom network**.
{% endstep %}

{% step %}
#### Enter the network details

| Field              | Value                                                                        |
| ------------------ | ---------------------------------------------------------------------------- |
| Network name       | Elysium Mainnet                                                              |
| Default RPC URL    | `https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/`                    |
| Chain ID           | 99901                                                                        |
| Currency symbol    | HYPE                                                                         |
| Block explorer URL | [https://elysium.kinetiq.xyz/explorer](https://elysium.kinetiq.xyz/explorer) |
{% endstep %}

{% step %}
#### Save and switch

Save the network and select it. MetaMask will route all JSON-RPC calls through your GetBlock endpoint.
{% endstep %}
{% endstepper %}

{% hint style="info" %}
Replace `<ACCESS-TOKEN>` with your GetBlock access token. Keep the token private — anyone with it can send requests against your quota.
{% endhint %}

### Deploy with Foundry

{% stepper %}
{% step %}
#### Install Foundry

```bash
curl -L https://foundry.paradigm.xyz | bash
foundryup
```
{% endstep %}

{% step %}
#### Create a project

```bash
forge init hello-elysium && cd hello-elysium
```
{% endstep %}

{% step %}
#### Write a contract

{% code title="src/Storage.sol" %}
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.19;

contract Storage {
    uint256 public value;

    function set(uint256 newValue) external {
        value = newValue;
    }
}
```
{% endcode %}
{% endstep %}

{% step %}
#### Deploy

```bash
forge create src/Storage.sol:Storage \
  --rpc-url https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/ \
  --private-key <YOUR_PRIVATE_KEY> \
  --broadcast
```

Foundry compiles the contract, estimates gas via `eth_estimateGas`, and submits the deployment through `eth_sendRawTransaction`. The transaction is priced in HYPE.
{% endstep %}
{% endstepper %}

### Deploy with Hardhat

{% stepper %}
{% step %}
#### Set up the project

```bash
mkdir hello-elysium && cd hello-elysium
npm init -y
npm install --save-dev hardhat
npx hardhat init
```
{% endstep %}

{% step %}
#### Configure the network

{% code title="hardhat.config.js" %}
```javascript
require("@nomicfoundation/hardhat-toolbox");

module.exports = {
  solidity: "0.8.19",
  networks: {
    elysium: {
      url: "https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/",
      chainId: 99901,
      accounts: ["<YOUR_PRIVATE_KEY>"]
    }
  }
};
```
{% endcode %}
{% endstep %}

{% step %}
#### Deploy

```bash
npx hardhat ignition deploy ./ignition/modules/Storage.js --network elysium
```
{% endstep %}
{% endstepper %}

### Verify the Deployment

Confirm the contract is live by reading its code through your GetBlock endpoint:

{% code overflow="wrap" %}
```bash
curl -X POST https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/ \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","method":"eth_getCode","params":["<CONTRACT_ADDRESS>","latest"],"id":"getblock.io"}'
```
{% endcode %}

A non-`0x` result confirms deployed bytecode at the address.

{% hint style="info" %}
**Source-code verification.** Elysium's explorer tooling (Kinetiq) is where you publish and verify source. Because the mainnet explorer URL and its verification backend (Blockscout-style or otherwise) should be confirmed from the official docs, this guide does not hard-code a verification command. Check [elysium.kinetiq.xyz/docs](https://elysium.kinetiq.xyz/docs) for the current verification flow.
{% endhint %}

### Support

For technical support and questions:

* Support: [support@getblock.io](mailto:support@getblock.io)
