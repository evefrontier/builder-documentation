# World contract upgrades

[EVE Frontier world-contracts](https://github.com/evefrontier/world-contracts) ships as versioned releases. Each **upgrade** creates a **new package object** (new address); older versions stay on-chain and stay callable.

---

## How package upgrades work on Sui

Read these in order:

1. **[Object and package versioning](https://docs.sui.io/guides/developer/objects/versioning)** — User packages get a **new package ID per publish/upgrade**; the family is tied by **original ID** and **UpgradeCap**.
2. **[Upgrading packages](https://docs.sui.io/build/package-upgrades)** — Compatibility rules, `UpgradeCap` / tickets, `sui client upgrade`, old packages remain on-chain.
3. **[Move package management](https://docs.sui.io/guides/developer/sui-101/move-package-management)** — `published-at`, dependencies, upgraded dependencies.

---

## Where the IDs live

After a world upgrade, use **`published-at`** (per environment) as the default for new transactions and for tooling that resolves module paths—unless you intentionally depend on an older version.

In [world-contracts](https://github.com/evefrontier/world-contracts), each environment is recorded in `contracts/world/Published.toml`:

- **`original-id`** — First package in the upgrade family (the original publish address).
- **`published-at`** — **Latest** package ID; use for app config and **new** world APIs when you want current bytecode.

Bump your Move dependency to the release tag you need (e.g. `v0.0.20`), refresh `Move.lock`, then republish or **upgrade** your extension with your `UpgradeCap` if the world API you use changed.

### TypeScript / dApps

Use a configurable `packageId` in `moveCall` targets (see [Interfacing with the EVE Frontier World](interfacing-with-the-eve-frontier-world.md)). After an upgrade, set `packageId` from **`published-at`** for that network, rebuild, and redeploy.

### MVR (future)

[Move Registry (MVR)](https://docs.suins.io/move-registry) resolves a stable name (e.g. `@evefrontier/world`) to the **latest** package and helps canonicalize types. TS SDK tooling can bake in MVR resolution at **build** time. Operator setup: [MVR setup in world-contracts](https://github.com/evefrontier/world-contracts/blob/main/docs/mvr-setup.md).

---

## Migration checklist

If you are still pinned to an **old** world package ID:

| Audience | What to do |
|----------|------------|
| **dApps** | Point config at **`published-at`** when you need new functions or fixes. |
| **Move extensions** | Bump world-contracts, test, upgrade your package if needed. |
| **Indexers** | Do not assume a single type prefix across upgrades, expand filters or use canonical type resolution ([GraphQL `type`](https://docs.sui.io/references/sui-api/sui-graphql/beta/reference/operations/queries/type); see also [MystenLabs/sui#12853](https://github.com/MystenLabs/sui/pull/12853) for event edge cases). |

---

## Which package ID for what?

| Situation | Package ID |
|-----------|------------|
| **New functions** (only in upgraded bytecode) | **Latest** (`published-at`) in `moveCall` targets. |
| **Existing flows** (unchanged public APIs) | **Original** or **latest**—both work. |
| **Object lookups / type filters** (e.g. `getOwnedObjects`) | Use the **package ID in the object’s type string** (often **original** for objects from before the upgrade, e.g. `originalId::gate::JumpPermit`). Types are not auto-retagged. |

**Example:** A new **invalidate** entrypoint ships on the upgraded package(v0.0.20). **Issue JumpPermit** can use **latest** or **original**; **invalidate** must use **latest**.

**GraphQL:** [`type`](https://docs.sui.io/references/sui-api/sui-graphql/beta/reference/operations/queries/type) accepts a type string with any package address in the lineage **at or after** the **defining** (first) package for that type and returns the canonical form—useful when comparing types across upgrades.
