---
description: >-
  This guide covers adding the GIWA network to a browser wallet, then deploying
  a smart contract to GIWA using Foundry, Hardhat, or Remix.
---

# Deploy a Smart Contract - GIWA

GIWA is fully EVM-equivalent, so contracts deploy with the standard Solidity toolchain — Foundry, Hardhat, or Remix — pointed at a GetBlock endpoint. This guide covers adding the network to a wallet and deploying a first contract to GIWA.

## Prerequisites

* An EVM wallet (such as MetaMask) with GIWA ETH for gas — see [Add Network to Your Wallet](#add-network-to-your-wallet) and [Testnet Faucets](#testnet-faucets)
* A GetBlock access token from the [GetBlock dashboard](https://getblock.io) — used as `<ACCESS-TOKEN>` in the RPC URL
* Node.js 20+ (required for Hardhat)
* A funded deployer address with GIWA ETH (see [Testnet Faucets](#testnet-faucets))

### Network Details

| Property | GIWA |
| -------- | --------- |
| Chain ID | 91342 (0x164ce) |
| RPC URL | `https://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/` |
| Currency Symbol | ETH |
| Block Explorer | [sepolia-explorer.giwa.io](https://sepolia-explorer.giwa.io) |

{% hint style="info" %}
Ethereum Mainnet and Sepolia are pre-configured in MetaMask by default. GIWA is not, so it must be added manually — use the button below or the manual steps that follow.
{% endhint %}

{% hint style="warning" %}
This guide targets GIWA (a test network). Deploy and exercise contracts here before deploying to any production network. Never commit a real private key: use environment variables and a throwaway deployer key for testing. Private keys committed to a public repository are drained within seconds by automated bots.
{% endhint %}

## Add Network to Your Wallet

### Automatic

{% hint style="info" %}
The block below is HTML for embedding via GitBook's Custom HTML block. Replace `<ACCESS-TOKEN>` in the RPC URL before publishing.
{% endhint %}

{% code title="add-network-to-metamask.html" overflow="wrap" %}

```html
<div style="display: flex; gap: 12px; flex-wrap: wrap;">
  <button
    style="background: #6b7280; color: white; border: 0; padding: 10px 20px; border-radius: 6px; cursor: pointer; font-weight: 600;"
    onclick="addNetwork('0x164ce', 'GIWA', 'https://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/', 'https://sepolia-explorer.giwa.io')">
    Add GIWA
  </button>
</div>

<script>
async function addNetwork(chainIdHex, chainName, rpcUrl, explorerUrl) {
  if (typeof window.ethereum === 'undefined') {
    alert('No EVM wallet detected. Install MetaMask or a compatible EIP-3085 wallet first.');
    return;
  }
  try {
    await window.ethereum.request({
      method: 'wallet_addEthereumChain',
      params: [{
        chainId: chainIdHex,
        chainName: chainName,
        nativeCurrency: { name: 'Ether', symbol: 'ETH', decimals: 18 },
        rpcUrls: [rpcUrl],
        blockExplorerUrls: [explorerUrl]
      }]
    });
  } catch (err) {
    console.error('Failed to add network:', err);
    alert('Failed to add network: ' + (err.message || err));
  }
}
</script>
```

{% endcode %}

### Manual

1. Open MetaMask (or your EVM wallet of choice).
2. Click the network dropdown at the top of the wallet.
3. Click **Add network** → **Add a network manually**.
4. Enter the network details from the [Network Details](#network-details) table above.
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

## Testnet Faucets

### GIWA

GIWA gas is ETH bridged from Ethereum Sepolia. First obtain Sepolia ETH, then bridge it to GIWA.

Obtain Sepolia ETH:

* [Google Cloud Web3 Faucet](https://cloud.google.com/application/web3/faucet/ethereum/sepolia) — 0.05 ETH per day per Google account
* [Alchemy Sepolia Faucet](https://www.alchemy.com/faucets/ethereum-sepolia) — requires a free Alchemy account
* [PK910 Sepolia PoW Faucet](https://sepolia-faucet.pk910.de/) — earn Sepolia ETH via in-browser proof-of-work mining

Bridge Sepolia ETH to GIWA:

* Use the official GIWA bridge linked from the [GIWA documentation](https://docs.giwa.io) to move Sepolia ETH to GIWA (confirm the current bridge URL in the GIWA docs before publishing) *(verify)*
