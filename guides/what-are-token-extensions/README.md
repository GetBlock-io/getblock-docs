---
description: >-
  Solana Token Extensions: guide to types, use cases & why they matter for SPL
  tokens
---

# What Are Token Extensions

The Solana Program Library (SPL) Token program defines a standard for creating, minting, transferring, and burning both fungible and non-fungible tokens, similar to the ERC-20 and ERC-721 standards in Ethereum. This program has a restricted definition, hence requiring developers to build custom code to add functionality beyond what is originally provided. This is time-consuming and difficult to achieve adoption across the ecosystem.&#x20;

Solana's programming model requires that programs be included in transactions alongside accounts, making it complicated to craft transactions involving multiple token programs. Additionally, each forked version introduces unaudited code, raising security concerns.

Then comes the Token Extension (Token-2022). This is a more advanced token program that maintains the original functionalities while expanding its capacity beyond the fundamentals (e.g., token transfers, minting, etc.). The difference lies in the optional extensions you can enable to add new capabilities natively.

### Why Token Extensions Matter

Token Extensions address a growing need: **permissioned tokens on a permissionless network**. This makes Solana viable for:

* **Institutional adoption** – Banks, asset managers, and regulated entities (e.g., Paxos with USDP) can issue compliant tokens with built-in controls like clawback capabilities and KYC-gated transfers
* **Real-World Asset (RWA) tokenization** – Securities, bonds, and other regulated assets require features like transfer restrictions and mandatory compliance checks
* **Creator economies** – NFT royalties, transfer taxes, and holder rewards can be enforced at the protocol level rather than relying on marketplace cooperation

### Available Extensions

Token Extensions are divided into two categories:

#### Mint Extensions (Applied to Token Mints)

| Extension                   | Description                                                                                                       | Potential Use Cases                                                                                                      | Real-World Example                                    |
| --------------------------- | ----------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------- |
| Confidential Transfers      | Protects the confidentiality of user balances within a transfer, as well as hiding transaction amounts            | Onchain payroll, B2B payments, treasury management                                                                       | [Paxos USDP](https://www.paxos.com/)                  |
| Transfer Fees               | The ability to charge fees at the protocol level; fees are withheld from recipient                                | Permanent royalties, publisher fees, transaction fees                                                                    | [BERN](https://www.bonkcoin.com/) (6.9% transfer fee) |
| Transfer Hooks              | Gives the token issuer control over which wallets can interact with their token and how tokens and users interact | KYC verification, token-gated access, royalty enforcement                                                                | [WEN (WNS)](https://www.wen.tools/)                   |
| Metadata & Metadata Pointer | Establishes verifiable links between tokens and metadata directly on mint accounts                                | Token verification, accounts receivable attribution                                                                      | [WEN (WNS)](https://www.wen.tools/)                   |
| Interest-Bearing Tokens     | Adjusts displayed token amounts over time (cosmetic, not actual minting)                                          | Yield-bearing stablecoins, bonds, savings products                                                                       |                                                       |
| Non-Transferable Tokens     | Makes it impossible to reassign the owner of a token, except by issuer                                            | Managing an external database, non-transferable mints of NFTs, soul-bound credentials                                    |                                                       |
| Permanent Delegate          | Allows a program to have irrevocable authority over a token                                                       | Automatic subscription services, updating real world assets, stablecoin regulatory compliance with freeze & seize orders | [Paxos USDP](https://paxos.com/)                      |
| Default Account State       | Configure and enforce token account permissions (frozen by default)                                               | KYC verification, compliance gating                                                                                      |                                                       |
| Close Mint                  | Allows closing mint accounts when supply reaches 0                                                                | Temporary tokens, limited editions                                                                                       |                                                       |
| Token Groups and Members    | Enables grouping tokens together                                                                                  | NFT collections, token families                                                                                          | [WEN (WNS)](https://www.wen.tools/)                   |
| Pausable Mint               | Allows pausing all token operations globally                                                                      | Emergency stops, maintenance windows                                                                                     |                                                       |

#### Account Extensions (Applied to Token Accounts)

| Extension       | Description                                               | Potential Use Cases                                       |
| --------------- | --------------------------------------------------------- | --------------------------------------------------------- |
| Memo Transfer   | Requires a memo instruction before all incoming transfers | Payment attribution, compliance records, invoice tracking |
| Immutable Owner | Prevents the token account owner from being changed       | Security, preventing account hijacking                    |
| CPI Guard       | Protects against certain cross-program invocation attacks | Security for high-value accounts                          |

{% hint style="warning" %}
### Extension Compatibility

Not all extensions can be combined. Some conflicts include:<br>

* Non-Transferable + Transfer Hooks: If tokens can't transfer, a hook has no work to do
* Non-Transferable + Transfer Fees: No transfers means no fees to collect
* Confidential Transfers + Transfer Hooks: Hooks need to read transfer amounts, but confidential transfers encrypt them
{% endhint %}

### Next Steps

* [Learn about how to create and Mint token with Token-2022 and GetBlock Solana RPC](how-to-create-tokens-with-metadata-on-solana-using-token-2022.md)
