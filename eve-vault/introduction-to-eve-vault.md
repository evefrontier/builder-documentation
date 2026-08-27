# Introduction to EVE Vault

**EVE Vault** is the official wallet and inventory manager for EVE Frontier on Sui, available as a [web app and extensions for Chrome and Firefox](https://github.com/evefrontier/evevault). For what it is, how to install it, and how wallet and identity work (including zkLogin), see [Wallets & Identity](wallets-and-identity.md).

**Features:**

* Sui Wallet Standard compliance for fast, safe, cross-dApp integration.
* FusionAuth OAuth support for linking your EVE Frontier account and in-game character to your wallet.
* Permissioned dApp access: connect to player-made dApps and tools while protecting your identity — even if your clone is destroyed.

***

## The Economy and Currencies

The EVE Frontier economy spans an off-chain, in-game currency and an on-chain Sui token, connected so value can move between in-game and on-chain contexts.

* **LUX**: The off-chain, in-game currency used for most in-game transactions, purchases, trades, and services. LUX is tracked in-game and is not a Sui coin.
* **EVE Token**: The native on-chain token of EVE Frontier (a `Coin<EVEToken>` on Sui). It powers the open, composable EVE Frontier game and the builder-driven economy.

LUX and EVE Token are linked by an exchange service that converts between the off-chain LUX currency and the on-chain EVE Token in both directions, letting value move between the game and the Sui blockchain.

EVE Vault enables:

* Trading and storage of EVE Token and other Sui-based assets.
* Secure management and transfer of player-generated and alliance-specific currencies.
* Integration with markets, trading systems, and configurable infrastructure rewards.

**Core advantages:**

* Trade assets, currencies, services, and reputation with robust permissions and privacy.
* Create, exchange, and use custom currencies—including alliance tokens and specialized mission rewards.
* Seamless experience for builders, traders, and players operating across the decentralized EVE Frontier world.

***
