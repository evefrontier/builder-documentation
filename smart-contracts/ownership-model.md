# Ownership Model

EVE Frontier uses a **capability-based** access control system. Instead of relying on wallet addresses for ownership, transferable capability objects (`OwnerCap`) grant access to on-chain objects.

## Access Hierarchy

```
GovernorCap  (deployer — top-level authority)
    └── AdminACL  (shared object — authorized sponsor addresses)
            └── OwnerCap<T>  (player — mutates a specific object)
```

- **`GovernorCap`** — created at deploy time. Can add/remove sponsors in `AdminACL`.
- **`AdminACL`** — a shared object containing a list of authorized sponsor addresses. Functions protected by `AdminACL` call `verify_sponsor(ctx)`, which checks the transaction sponsor.
- **`OwnerCap<T>`** — a typed "keycard" that authorizes mutation of **one specific object**.

```move
public struct OwnerCap<phantom T> has key {
    id: UID,
    authorized_object_id: ID,
}
```

## Character as a Keychain

All `OwnerCap`s are stored inside the player's `Character` object (not in their wallet directly). This is inspired by a real-world **keychain pattern** — a single master key (the character) grants access to many capabilities.

```
User Wallet
    └── Character (shared object, mapped to user address)
            ├── OwnerCap<NetworkNode>
            ├── OwnerCap<Gate>
            ├── OwnerCap<StorageUnit>
            └── ...
```

When a Smart Assembly is created, its `OwnerCap` is minted and transferred to the `Character` object. If the user has access to the character, they have access to all its capabilities.

## Borrow-Use-Return Pattern

To use an `OwnerCap`, the player **borrows** it from the character, uses it, and **returns** it all within a single transaction. This uses Sui's [`Receiving`](https://docs.sui.io/guides/developer/objects/transfers/transfer-to-object) pattern.

```move
// 1. Borrow the OwnerCap from the character
public fun borrow_owner_cap<T: key>(
    character: &mut Character,
    owner_cap_ticket: Receiving<OwnerCap<T>>,
    ctx: &TxContext,
): (OwnerCap<T>, ReturnOwnerCapReceipt)

// 2. Use it (e.g., bring a storage unit online)
public fun online(
    storage_unit: &mut StorageUnit,
    network_node: &mut NetworkNode,
    energy_config: &EnergyConfig,
    owner_cap: &OwnerCap<StorageUnit>,
)

// 3. Return it to the character
public fun return_owner_cap<T: key>(
    character: &Character,
    owner_cap: OwnerCap<T>,
    receipt: ReturnOwnerCapReceipt,
)
```

A `ReturnOwnerCapReceipt` (hot potato) ensures the cap is always returned or explicitly transferred, it cannot be silently dropped.

### TypeScript Example

A typical owner-authenticated call (e.g., bringing a network node online):

```typescript
// 1. Borrow OwnerCap from character
const [ownerCap, receipt] = tx.moveCall({
  target: `${packageId}::character::borrow_owner_cap`,
  typeArguments: [`${packageId}::network_node::NetworkNode`],
  arguments: [tx.object(characterId), tx.object(ownerCapId)],
});

// 2. Use OwnerCap to bring network node online
tx.moveCall({
  target: `${packageId}::network_node::online`,
  arguments: [tx.object(nwnId), ownerCap, tx.object(CLOCK_OBJECT_ID)],
});

// 3. Return OwnerCap to character
tx.moveCall({
  target: `${packageId}::character::return_owner_cap`,
  arguments: [tx.object(characterId), ownerCap, receipt],
});
```

## Benefits

- **Centralized ownership** — manage all capabilities from a single character object
- **Granular access** — each `OwnerCap<T>` only authorizes one specific object
- **Delegatable** — transfer an `OwnerCap` without moving the underlying assembly
- **Composable** — the borrow-use-return pattern works within programmable transactions

**Reference:**
- [`access_control.move`](https://github.com/evefrontier/world-contracts/blob/main/contracts/world/sources/access/access_control.move)
- [`character.move`](https://github.com/evefrontier/world-contracts/blob/main/contracts/world/sources/character/character.move)
