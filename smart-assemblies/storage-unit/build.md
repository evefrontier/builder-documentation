# Build a Custom Storage Unit Extension

Step-by-step instructions for building a custom Storage Unit extension. For concepts and design, see the [Storage Unit README](./README.md).

## Prerequisites

- Follow [environment-setup](../../quickstart/environment-setup.md)
- Complete the step-by-step instructions for the `builder-scaffold`: [builder-scaffold builder-flow](https://github.com/evefrontier/builder-scaffold/blob/main/docs/builder-flow.md)

## Extension Integration

For extension development and integration follow the [high-level build steps](../introduction#high-level-build-steps).

### Storage Unit API overview

The [world storage unit module](https://github.com/evefrontier/world-contracts/blob/main/contracts/world/sources/assemblies/storage_unit.move) exposes:

- **Lifecycle & ownership:** Owner-cap–guarded `online` / `offline`, metadata updates, and admin flows (anchor, unanchor, energy source). View functions: `status`, `location`, `owner_cap_id`, `energy_source_id`, `inventory`, `is_extension_frozen`, `has_open_storage`, etc.

- **Inventories:** Storage units have multiple inventory slots keyed by ID:
  - **Primary (owner) inventory** — keyed by the storage unit’s `owner_cap_id`; owner uses `deposit_by_owner` / `withdraw_by_owner` (sender must be the character’s `character_address`).
  - **Owned (per-character) inventories** — created on demand, keyed by a character’s `OwnerCap` ID; extension can deposit into them via `deposit_to_owned<Auth>` (recipient need not be sender).
  - **Open inventory** — a standalone native slot (key from `open_storage_key`) that only the **registered extension** can use. Uses `deposit_to_open_inventory<Auth>` and `withdraw_from_open_inventory<Auth>` for interaction. Owners and players deposit and withdraw to this inventory only via the extension (e.g. vending, contract-controlled pool). Created on `anchor` or first `deposit_to_open_inventory`; use `has_open_storage` to check.

- **Extension authorization & freeze:** Owner calls `authorize_extension<Auth>` to register a witness type. Optionally, owner can call `freeze_extension_config` (Storage Unit supports this) so the extension can never be changed — irreversible, gives users guarantees about the extension’s operating logic. See [Extension freeze (optional)](../introduction#extension-freeze-optional).

- **Bridging:** `game_item_to_chain_inventory` (admin + owner), `chain_item_to_game_inventory` (owner + location proof).

## Smart Storage Unit Extension API

Custom contracts use the **typed witness pattern**: define a witness struct (`Auth`) and register it on the storage unit. The [world storage unit module](https://github.com/evefrontier/world-contracts/blob/main/contracts/world/sources/assemblies/storage_unit.move) verifies the type at runtime.

**Authorize an extension:**

```move
public fun authorize_extension<Auth: drop>(
    storage_unit: &mut StorageUnit,
    owner_cap: &OwnerCap<StorageUnit>,
)
```

**Freeze extension config (optional, irreversible):** Owner can call this after authorizing an extension so the extension cannot be changed.

```move
public fun freeze_extension_config(
    storage_unit: &mut StorageUnit,
    owner_cap: &OwnerCap<StorageUnit>,
)
```

**Extension: primary owner inventory deposit / withdraw** (your extension calls these with your `Auth` witness):

```move
public fun deposit_item<Auth: drop>(
    storage_unit: &mut StorageUnit,
    character: &Character,
    item: Item,
    _: Auth,
    _: &mut TxContext,
)

public fun withdraw_item<Auth: drop>(
    storage_unit: &mut StorageUnit,
    character: &Character,
    _: Auth,
    type_id: u64,
    quantity: u32,
    ctx: &mut TxContext,
): Item
```

**Extension: deposit into a player’s owned inventory** — Lets the extension push items into a specific character’s owned inventory; the recipient does not need to be the transaction sender (async delivery, guild hangars, rewards):

```move
public fun deposit_to_owned<Auth: drop>(
    storage_unit: &mut StorageUnit,
    character: &Character,
    item: Item,
    _: Auth,
    _: &mut TxContext,
)
```

**Extension: open inventory** — Contract-controlled inventory slot; only the registered extension can deposit or withdraw. Owners and players get items out only via the extension (e.g. `withdraw_from_open_inventory` in your logic):

```move
public fun deposit_to_open_inventory<Auth: drop>(
    storage_unit: &mut StorageUnit,
    character: &Character,
    item: Item,
    _: Auth,
    _: &mut TxContext,
)

public fun withdraw_from_open_inventory<Auth: drop>(
    storage_unit: &mut StorageUnit,
    character: &Character,
    _: Auth,
    type_id: u64,
    quantity: u32,
    ctx: &mut TxContext,
): Item
```

**Owner deposit / withdraw** (no extension; owner uses `OwnerCap`; sender must be the character’s `character_address`):

```move
public fun deposit_by_owner<T: key>(
    storage_unit: &mut StorageUnit,
    item: Item,
    character: &Character,
    owner_cap: &OwnerCap<T>,
    ctx: &mut TxContext,
)

public fun withdraw_by_owner<T: key>(
    storage_unit: &mut StorageUnit,
    character: &Character,
    owner_cap: &OwnerCap<T>,
    type_id: u64,
    quantity: u32,
    ctx: &mut TxContext,
): Item
```

---

<!-- TODO: When builder-scaffold has a full Storage Unit extension example (like smart_gate), add sections here: 1. Understand the example contract, 2. Build and publish, 3. Configure extension rules, 4. Authorize the extension, 5–6. Example flows (deposit/withdraw, open inventory). For now use the Gate build guide and builder-scaffold as reference. -->

*Builder-scaffold example flow for Storage Unit will be documented here once available. Until then, use the [Gate build guide](../gate/build.md) and [builder-scaffold](https://github.com/evefrontier/builder-scaffold) for publish, authorize, and borrow-use-return patterns.*

## Reference

- [world-contracts](https://github.com/evefrontier/world-contracts) — EVE Frontier Sui Move contracts
- [storage_unit.move](https://github.com/evefrontier/world-contracts/blob/main/contracts/world/sources/assemblies/storage_unit.move) — core storage unit module
- [extension_freeze.move](https://github.com/evefrontier/world-contracts/blob/main/contracts/world/sources/assemblies/extension_freeze.move) — extension freeze (Storage Unit, Gate, Turret)
- [inventory.move](https://github.com/evefrontier/world-contracts/blob/main/contracts/world/sources/primitives/inventory.move) — inventory primitives
- [contracts/world](https://github.com/evefrontier/world-contracts/tree/main/contracts/world) — world contract package
