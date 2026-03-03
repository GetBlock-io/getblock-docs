---
description: >-
  Learn how to add tips to private transactions submitted via GetBlock's
  bloxRoute integration.
---

# Sending Transactions to Private Mempool (Priority Fee)

Priority fees incentivize builders to include your transaction faster and position it more favorably within a block. Adding a tip to your private transaction provides three key benefits:

* **Higher inclusion probability:** Builders prioritize transactions with higher fees
* **Better block positioning:** Achieve positions 1–2 more reliably
* **Faster confirmation:** Reduce waiting time for transaction inclusion

## Choosing a Method

They are Two approaches exist for adding priority fees to private transactions:

| Method                                     | Best For                                           | Trade-offs                                          |
| ------------------------------------------ | -------------------------------------------------- | --------------------------------------------------- |
| [**Multicall3**](how-to-use-multicall3.md) | Most use cases                                     | Single nonce, atomic execution, slightly higher gas |
| [**Bundle**](how-to-use-bundle.md)         | Advanced scenarios requiring separate transactions | Two nonces, more complex setup                      |

### Different Between Bundles and  Transactions

Choose the appropriate method based on your use case:

| **Method**                | `bsc_privateTx`                    | `mev_sendBundle`                 |
| ------------------------- | ---------------------------------- | -------------------------------- |
| **Transaction count**     | Single                             | Multiple                         |
| **Atomicity**             | Not applicable                     | All-or-nothing execution         |
| **Use cases**             | Simple transfers, individual swaps | Arbitrage, multi-step operations |
| **Adding priority fees**  | Via Multicall3                     | Separate transaction in bundle   |
| **`mev_builders` format** | Array: `["all"]`                   | Object: `{"all": ""}`            |

### Recommended Tip Amounts

| Priority  | Tip Amount | Use Case                |
| --------- | ---------- | ----------------------- |
| Low       | 0.0001 BNB | Regular trades          |
| Medium    | 0.0005 BNB | Time-sensitive          |
| High      | 0.001 BNB  | Arbitrage, liquidations |
| Very High | 0.005+ BNB | Critical, competitive   |

{% hint style="info" %}
**Note:** A higher tip means higher priority, but you only pay if TX is included.
{% endhint %}

### Next Steps

Learn how to submit transactions via [Multicall3](how-to-use-multicall3.md) or [Bundle](how-to-use-bundle.md) method to the private mempool.&#x20;
