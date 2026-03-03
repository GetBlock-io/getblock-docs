---
description: >-
  Learn how to add tips to private transactions submitted via GetBlock's
  bloxRoute integration.
---

# Sending Private Transactions (Priority Fee)

Priority fees incentivize MEV builders to include your transaction faster and position it more favorably within a block. Adding a tip to your private transaction provides three key benefits:

* **Higher inclusion probability** — Builders prioritize transactions with higher fees
* **Better block positioning** — Achieve positions 1–2 more reliably
* **Faster confirmation** — Reduce waiting time for transaction inclusion

## Choosing a Method

They are Two approaches exist for adding priority fees to private transactions:

| Method                                                              | Best For                                           | Trade-offs                                          |
| ------------------------------------------------------------------- | -------------------------------------------------- | --------------------------------------------------- |
| [**Multicall3**](how-to-use-multicall3-for-private-transactions.md) | Most use cases                                     | Single nonce, atomic execution, slightly higher gas |
| [**Bundle**](how-to-use-bundle-for-private-transaction.md)          | Advanced scenarios requiring separate transactions | Two nonces, more complex setup                      |

{% hint style="success" %}
**Recommendation:** Use Multicall3 for most applications. It guarantees atomic execution and simplifies nonce management.
{% endhint %}

#### Comparison: Multicall vs Bundle

| Aspect            | Bundle (2 TX)        | Multicall (1 TX)      |
| ----------------- | -------------------- | --------------------- |
| Nonce consumption | 2 nonces             | **1 nonce**           |
| Atomicity         | Depends on builder   | **Guaranteed**        |
| API Method        | `blxr_submit_bundle` | `bsc_private_tx`      |
| Fee payment       | Separate TX          | **Internal transfer** |
| Complexity        | Higher               | **Simpler**           |

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

### Additional Resources

* [Multicall3 Contract Documentation](https://github.com/mds1/multicall)
* [BloXroute BSC Documentation](https://docs.bloxroute.com/)
* [BSCScan Explorer](https://bscscan.com/)
