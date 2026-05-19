# World contract upgrades

[EVE Frontier world-contracts](https://github.com/evefrontier/world-contracts) ships as versioned releases. Each **upgrade** creates a **new package object** (new address); older versions stay on-chain and stay callable.

---

## How package upgrades work on Sui

Read these in order:

1. **[Object and package versioning](https://docs.sui.io/guides/developer/objects/versioning)** — User packages get a **new package ID per publish/upgrade**; the family is tied by **original ID** and **UpgradeCap**.
2. **[Upgrading packages](https://docs.sui.io/build/package-upgrades)** — Compatibility rules, `UpgradeCap` / tickets, `sui client upgrade`, old packages remain on-chain.
3. **[Move package management](https://docs.sui.io/guides/developer/sui-101/move-package-management)** — `published-at`, dependencies, upgraded dependencies.

---

## Using MVR (recommended for clients)

The [Move Registry (MVR)](https://docs.suins.io/move-registry) maps a **stable application name** to the **correct package address** for the network your client uses. After we publish an upgrade, you keep the same MVR name in config and tooling; resolution picks up the **latest** registered package for that name on **testnet** or **mainnet**, instead of chasing two hex IDs.

`@evefrontier/world`  : [https://www.moveregistry.com/package/@evefrontier/world?tab=versions](https://www.moveregistry.com/package/@evefrontier/world?tab=versions)

Today this name resolves to our **testnet** deployment for Stillness server. When Stillness ships on mainnet and the registry is updated, the **same name** will resolve to the production package on mainnet and to the sandbox/testnet package when clients target testnet. 

**Integrate in TypeScript:** [Transaction plugin for Sui TypeScript SDK](https://docs.suins.io/move-registry/tooling/typescript-sdk) (`namedPackagesPlugin`, or `@mysten/sui` client resolution). Use `@evefrontier/world::…` in `moveCall` `target` strings and in type references the same way you would a package ID.

### Why this replaces the old “two package IDs” mental model (for apps)

Before MVR, builders often tracked **two** hex addresses from our `Published.toml`:

- **`published-at`** — latest package ID; needed for **new** entrypoints and bytecode that only exist on the upgraded package.
- **`original-id`** — first package in the family; objects and type strings created before an upgrade often still reference this id in **on-chain type tags** and filters.

For **calling** the world package from a dApp, you had to update `packageId` in config when we shipped a new `published-at`. With MVR, **one name** (`@evefrontier/world`) is enough for those call targets on a given network, as long as your client resolves names against the right chain.

You still need to understand **original vs latest** when **reading** existing objects or filtering by type string; see [Which package ID for what?](#which-package-id-for-what) below.

---

## Where the IDs live (Move / extensions / debugging)

After a world upgrade, **`published-at`** (per environment) remains the **latest** package ID in the repo and in `Published.toml`. Extension authors and anyone editing `Move.toml` still care about **`original-id`** vs **`published-at`** for dependency pins and upgrades.

In [world-contracts](https://github.com/evefrontier/world-contracts), each environment is recorded in `contracts/world/Published.toml`:

- **`original-id`** — First package in the upgrade family (the original publish address).
- **`published-at`** — **Latest** package ID; used for Move dependency `published-at`, extension upgrades, and **manual** hex-based tooling.

Bump your Move dependency to the release tag you need (e.g. `v0.0.21`), refresh `Move.lock`, then republish or **upgrade** your extension with your `UpgradeCap` if the world API you use changed.

### TypeScript / dApps

Prefer **`@evefrontier/world`** (MVR) for configurable `moveCall` targets when your stack supports it. If you are not using MVR yet, use a configurable `packageId` set from **`published-at`** for that network.

---

## Migration checklist

If you are still pinned to an **old** world package ID:


| Audience              | What to do                                                                                                                                                                                                                                                                                                             |
| --------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **dApps**             | Prefer **`@evefrontier/world`** (MVR) for call targets on testnet; otherwise point config at **`published-at`** when you need new functions or fixes.                                                                                                                                                                  |
| **Custom extensions** | Bump world-contracts, test, upgrade your package if needed.                                                                                                                                                                                                                                                            |
| **Indexers**          | Do not assume a single type prefix across upgrades, expand filters or use canonical type resolution ([GraphQL `type`](https://docs.sui.io/references/sui-api/sui-graphql/beta/reference/operations/queries/type)); see also [MystenLabs/sui#12853](https://github.com/MystenLabs/sui/pull/12853) for event edge cases). |


---

## Which package ID for what?


| Situation                                                      | What to use                                                                                                                                                                    |
| -------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **`moveCall` targets from an app (recommended)**               | **`@evefrontier/world`** — resolves to the latest registered package on the network your RPC targets (Stillness / testnet today).                                            |
| **New functions** (only in upgraded bytecode), **without** MVR | **Latest** (`published-at`) in `moveCall` targets.                                                                                                                             |
| **Existing flows** (unchanged public APIs), **without** MVR    | **Original** or **latest**—both work.                                                                                                                                          |
| **Object lookups / type filters** (e.g. `getOwnedObjects`)     | Use the **package ID in the object’s type string** (often **original** for objects from before the upgrade, e.g. `originalId::gate::JumpPermit`). Types are not auto-retagged. |


**GraphQL:** `[type](https://docs.sui.io/references/sui-api/sui-graphql/beta/reference/operations/queries/type)` accepts a type string with any package address in the lineage **at or after** the **defining** (first) package for that type and returns the canonical form—useful when comparing types across upgrades.