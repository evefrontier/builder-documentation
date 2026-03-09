# Build a Custom Storage Unit Extension

Step-by-step instructions for building a custom Storage Unit extension. For concepts and design, see the [Storage Unit README](./README.md).

## Prerequisites

- Follow [environment-setup](../../quickstart/environment-setup.md)
- For typed-witness and publish flow, see the [Gate build guide](../gate/build.md) and [builder-scaffold](https://github.com/evefrontier/builder-scaffold) — use them as the source of truth for setup and build steps.

## High-level steps

1. **Define a witness struct** (e.g. `public struct VendingAuth has drop {}`) in your Move package.
2. **Implement your logic** so it calls the [world storage unit module](https://github.com/evefrontier/world-contracts/blob/main/contracts/world/sources/assemblies/storage_unit.move) to deposit or withdraw items using your `Auth` witness (e.g. after checking payment, tribe, or other rules).
3. **Publish** the package and **authorize** it on the storage unit: borrow the storage unit’s `OwnerCap` and call `storage_unit::authorize_extension<YourAuth>`.

After authorization, only your extension’s logic can perform deposits/withdrawals for non-owner characters; the world module checks the `Auth` type.

## Storage Unit API

Custom contracts use the **typed witness pattern**: define a witness struct (`Auth`) and register it on the storage unit.

**Authorize an extension:**

```move
public fun authorize_extension<Auth: drop>(
    storage_unit: &mut StorageUnit,
    owner_cap: &OwnerCap<StorageUnit>,
)
```

**Extension: deposit / withdraw** (main inventory; your extension calls these with your `Auth` witness):

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

**Extension: deposit into a player’s owned inventory** Lets the extension push items into a specific character’s owned inventory; the recipient does not need to be the transaction sender. Use for async delivery, guild hangars, rewards:

```move
public fun deposit_to_owned<Auth: drop>(
    storage_unit: &mut StorageUnit,
    character: &Character,
    item: Item,
    _: Auth,
    _: &mut TxContext,
)
```

**Owner deposit / withdraw** (no extension; owner uses `OwnerCap`; sender must be the character).

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

Example use cases: vending machine (pay then withdraw), trade hub, gated access. See [builder-scaffold storage unit extension](https://github.com/evefrontier/builder-scaffold/tree/main/move-contracts/storage_unit_extension) for a working example.

## Reference

- [world-contracts](https://github.com/evefrontier/world-contracts) — EVE Frontier Sui Move contracts
- [storage_unit.move](https://github.com/evefrontier/world-contracts/blob/main/contracts/world/sources/assemblies/storage_unit.move) — core storage unit module
- [inventory.move](https://github.com/evefrontier/world-contracts/blob/main/contracts/world/sources/primitives/inventory.move) — inventory primitives
- [builder-scaffold storage_unit_extension](https://github.com/evefrontier/builder-scaffold/tree/main/move-contracts/storage_unit_extension) — example extension
- [contracts/world](https://github.com/evefrontier/world-contracts/tree/main/contracts/world) — world contract package