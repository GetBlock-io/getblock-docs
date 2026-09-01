---
description: >-
  This guide covers adding the GIWA network to a browser wallet, then deploying
  a smart contract to GIWA using Foundry, Hardhat, or Remix.
---

# Deploy a Smart Contract - GIWA

GIWA is fully EVM-equivalent, so contracts deploy using the standard Solidity toolchain — Foundry, Hardhat, or Remix — with a GetBlock endpoint. This guide covers adding the network to a wallet and deploying a first contract to GIWA.

## Prerequisites

* An EVM wallet (such as MetaMask) with GIWA ETH for gas — see [Add Network to Your Wallet](deploy-smart-contract-giwa.md#add-network-to-your-wallet) and [Testnet Faucets](deploy-smart-contract-giwa.md#testnet-faucets)
* A GetBlock access token from the [GetBlock dashboard](https://getblock.io) — used as `<ACCESS-TOKEN>` in the RPC URL
* Node.js 20+ (required for Hardhat)
* A funded deployer address with GIWA ETH&#x20;

### Network Details

| Property        | GIWA                                                         |
| --------------- | ------------------------------------------------------------ |
| Chain ID        | 91342 (0x164ce)                                              |
| RPC URL         | `https://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/`  |
| Currency Symbol | ETH                                                          |
| Block Explorer  | [sepolia-explorer.giwa.io](https://sepolia-explorer.giwa.io) |

{% hint style="info" %}
Ethereum Mainnet and Sepolia are pre-configured in MetaMask by default. GIWA is not, so it must be added manually — use the button below or the manual steps that follow.
{% endhint %}

{% hint style="warning" %}
This guide targets GIWA (a test network). Deploy and exercise contracts here before deploying to any production network. Never commit a real private key: use environment variables and a throwaway deployer key for testing. Private keys committed to a public repository are drained within seconds by automated bots.
{% endhint %}

## Add Network to Your Wallet

1. Open MetaMask (or your EVM wallet of choice).
2. Click the network dropdown at the top of the wallet.
3. Click **Add network** → **Add a network manually**.
4. Enter the network details from the [Network Details](deploy-smart-contract-giwa.md#network-details) table above.
5. Save and switch to the newly added GIWA network.

## Deploy with Foundry

1. **Install Foundry**

{% code overflow="wrap" %}
```bash
curl -L https://foundry.paradigm.xyz | bash
foundryup
```
{% endcode %}

2. **Create a project**

```bash
mkdir giwa-deploy && cd giwa-deploy
forge init
```

3. **Create a contract** — save as `src/HelloGiwa.sol`:

{% code title="src/HelloGiwa.sol" %}
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.30;

contract HelloGiwa {
    function hello() external pure returns (string memory) {
        return "Hello, GIWA";
    }
}
```
{% endcode %}

4. **Deploy** — export your credentials and broadcast:

{% code overflow="wrap" %}
```bash
export PRIVATE_KEY=0xyour_throwaway_test_key
export GIWA_RPC_URL=https://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/

forge create src/HelloGiwa.sol:HelloGiwa \
  --rpc-url $GIWA_RPC_URL \
  --private-key $PRIVATE_KEY \
  --broadcast
```
{% endcode %}

5. **Verify on the block explorer** — GIWA uses a Blockscout explorer:

{% code overflow="wrap" %}
```bash
forge verify-contract <CONTRACT_ADDRESS> src/HelloGiwa.sol:HelloGiwa \
  --chain-id 91342 \
  --verifier blockscout \
  --verifier-url https://sepolia-explorer.giwa.io/api/
```
{% endcode %}

## Deploy with Hardhat

1. **Create a project**

```bash
mkdir giwa-deploy && cd giwa-deploy
npm init -y
npm install --save-dev hardhat
npx hardhat init
```

2. **Configure the network** — populate `hardhat.config.js`:

{% code title="hardhat.config.js" %}
```javascript
require('@nomicfoundation/hardhat-toolbox');

const GIWA_SEPOLIA_CHAIN_ID = 91342;

module.exports = {
  solidity: '0.8.30',
  networks: {
    giwaSepolia: {
      url: 'https://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/',
      chainId: GIWA_SEPOLIA_CHAIN_ID,
      accounts: [process.env.PRIVATE_KEY]
    }
  },
  etherscan: {
    apiKey: { giwaSepolia: 'blockscout' },
    customChains: [
      {
        network: 'giwaSepolia',
        chainId: GIWA_SEPOLIA_CHAIN_ID,
        urls: {
          apiURL: 'https://sepolia-explorer.giwa.io/api/',
          browserURL: 'https://sepolia-explorer.giwa.io'
        }
      }
    ]
  }
};
```
{% endcode %}

3. **Create a contract** — save as `contracts/HelloGiwa.sol`:

{% code title="contracts/HelloGiwa.sol" %}
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.30;

contract HelloGiwa {
    function hello() external pure returns (string memory) {
        return "Hello, GIWA";
    }
}
```
{% endcode %}

4. **Compile and deploy** — save as `scripts/deploy.js`, then run it:

{% code title="scripts/deploy.js" %}
```javascript
const hre = require('hardhat');

async function main() {
  const contract = await hre.ethers.deployContract('HelloGiwa');
  await contract.waitForDeployment();
  console.log('HelloGiwa deployed to:', await contract.getAddress());
}

main().catch((error) => {
  console.error(error);
  process.exitCode = 1;
});
```
{% endcode %}

```bash
export PRIVATE_KEY=0xyour_throwaway_test_key
npx hardhat compile
npx hardhat run scripts/deploy.js --network giwaSepolia
```

5. **Verify on the block explorer**

```bash
npx hardhat verify --network giwaSepolia <CONTRACT_ADDRESS>
```
