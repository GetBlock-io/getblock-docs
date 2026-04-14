---
description: Learn how to calculate transaction fees in TRON
---

# TRON Fee Model

In Bitcoin and Ethereum, transaction fees are paid in the network’s native asset — in its smallest unit (satoshi, wei). TRON works differently; every transaction consumes two types of resources:

1. Bandwidth
2. Energy.&#x20;

If these resources are insufficient, TRX is burned to cover the costs.

#### SUN: The Base Unit of TRX

SUN is the smallest unit of TRX, analogous to **satoshi** in Bitcoin or **wei** in Ethereum.

```bash
1 TRX = 1,000,000 SUN
```

The cost of Energy and Bandwidth when burning TRX is expressed in SUN.

### Bandwidth

Bandwidth is the network throughput required to transmit transaction data.

* Consumed by every transaction
* 1 Bandwidth = 1 byte of transaction data
* Each account receives a free daily allowance of 600 Bandwidth, which resets automatically
* If Bandwidth is insufficient → TRX is burned

```bash
Cost when burning: 1 Bandwidth = 1,000 SUN (0.001 TRX)
```

#### Bandwidth Consumption Examples

| Operation             | Bandwidth Used  |
| --------------------- | --------------- |
| Sending TRX           | \~268 Bandwidth |
| Sending USDT (TRC-20) | \~345 Bandwidth |

TRC-10 token transfers consume only Bandwidth. TRC-20 token transfers (USDT and other smart contract tokens) consume both Bandwidth and Energy.

### Energy

Energy is the computational power required to execute smart contracts on the TRON Virtual Machine (TVM).

* Consumed only when calling a smart contract
* 1 Energy ≈ 1 microsecond of computation on TVM
* No free daily allowance
* If Energy is insufficient → TRX is burned

```bash
Cost when burning: 1 Energy = 100 SUN (0.0001 TRX)
```

#### Example: USDT (TRC-20) Transfer Energy Cost

A typical USDT transfer calls the function transfer(address to, uint256 value). The TVM executes a balance check, a storage write for the sender (debit), and a storage write for the recipient (credit).

The amount of Energy depends on whether a storage slot already exists for the recipient:

| Scenario                                     | Energy    | Reason                            |
| -------------------------------------------- | --------- | --------------------------------- |
| Activated address (has received USDT before) | \~65,000  | Updating an existing storage slot |
| Non-activated address (never received USDT)  | \~131,000 | Creating a new storage slot       |

### Transaction Cost When Burning TRX

Current network parameters (set via governance) can be retrieved with the following request:

```bash
GET https://api.trongrid.io/wallet/getchainparameters
Current rates: 1 Energy = 100 SUN (0.0001 TRX), 1 Bandwidth = 1,000 SUN (0.001 TRX)
```

#### Cost Calculation: USDT Transfer to Activated Address (\~65,000 Energy)

* Energy: 65,000 × 100 SUN = 6,500,000 SUN = 6.5 TRX
* Bandwidth: 345 × 1,000 SUN = 345,000 SUN = 0.345 TRX
* Total: \~6.85 TRX

#### Cost Calculation: USDT Transfer to New Address (\~131,000 Energy)

* Energy: 131,000 × 100 SUN = 13,100,000 SUN = 13.1 TRX
* Bandwidth: 345 × 1,000 SUN = 345,000 SUN = 0.345 TRX
* Total: \~13.45 TRX

### Where Energy Comes From: Staking and Delegation

TRON has a Stake 2.0 mechanism that allows users to obtain Energy without burning TRX:

#### 1. Staking (Freezing TRX)

A TRX holder freezes tokens via a system contract and receives Energy proportional to their share of the total network stake. The more TRX is frozen across the network, the less Energy each participant receives per unit of TRX. When unstaking, TRX is locked for a 14-day waiting period before it is returned.

#### 2. Delegation

An owner of staked TRX can delegate their resources (Energy or Bandwidth) to another address via delegateResource. The recipient uses the Energy, while the staker retains their TRX. Delegation can be locked for a minimum of 3 days, after which it can be revoked.

#### 3. Rental Market

This mechanism forms the basis of the Energy rental market: providers stake large amounts of TRX and delegate Energy to clients for a fee. The client receives Energy without having to lock up their own capital.

### Why Renting Energy Is More Cost-Effective Than Burning TRX

Rental providers buy Energy in bulk (by staking large amounts of TRX) and sell it at a price below the cost of burning. Current market prices range from 30 to 50 SUN per 1 Energy, depending on the provider and terms.

Example calculation at a rental price of 32 SUN per 1 Energy:

| Scenario                         | Renting   | Burning TRX | Savings |
| -------------------------------- | --------- | ----------- | ------- |
| Activated address (\~65K Energy) | \~2.1 TRX | 6.5 TRX     | \~68%   |
| New address (\~131K Energy)      | \~4.2 TRX | 13.1 TRX    | \~68%   |

For high-volume clients (exchanges, payment services, dApps), renting Energy significantly reduces operational costs.

### Energy Recovery

After being used, Energy on an account recovers linearly over 24 hours. This means that if an account has staked TRX and its Energy has been spent, the full amount will be available again after 24 hours. This also applies to rented (delegated) Energy — it is recovered on the recipient’s account during the rental period.

### Next Steps

1. [Pricing](/broken/pages/e7LJxCH0Sl7Chxp5lyqW)
2. [API Reference](api-reference/)
