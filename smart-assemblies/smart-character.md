# Smart Character

A Smart Character is the on-chain representation of an in-game character. It serves as the player's **identity on-chain** and the **owner of all assemblies** they create.

## Overview

When a player creates a character in-game, a corresponding `Character` object is created on-chain with the same character ID. The character is associated with a **tribe** and mapped to the player's **wallet address**.

## Character as Capability Holder

The character object acts as a **keychain** — it holds the `OwnerCap` for every object the player owns (network nodes, gates, storage units, etc.). See [Ownership Model](../smart-contracts/ownership-model#character-as-a-keychain) for details on the borrow-use-return pattern.

## Creation

Characters are created by the game server (admin) with a deterministic object ID derived from the in-game character ID.

## Discovering character from wallet address

The character object is a shared object; you need its object ID to interact with it. To get a character from a **wallet address** (e.g. when a player connects their wallet to your dApp), the game creates a **PlayerProfile** object at character creation and [transfers it to the wallet](https://github.com/evefrontier/world-contracts/pull/119). It contains only `character_id`. Query objects owned by the wallet with type `PlayerProfile` to obtain the character ID, then fetch the `Character` object.

For a full GraphQL query and variables, see [Query character by wallet address](../tools/interfacing-with-the-eve-frontier-world.md#query-character-by-wallet-address).

**IMPORTANT:** If the `PlayerProfile` object is not owned by the `character_address` defined in Character, character lookups by wallet address will not work. **DO NOT** transfer your `PlayerProfile` object. This is a stop-gap solution, and will change in the future.

## Access Control

Only the wallet address stored in `character_address` can borrow `OwnerCap`s from the character. This is enforced in the `borrow_owner_cap` function:

```move
public fun borrow_owner_cap<T: key>(
    character: &mut Character,
    owner_cap_ticket: Receiving<OwnerCap<T>>,
    ctx: &TxContext,
): (OwnerCap<T>, ReturnOwnerCapReceipt) {
    assert!(character.character_address == ctx.sender(), ESenderCannotAccessCharacter);
    // ...
}
```

## Transferring OwnerCaps to another account

You can transfer an **assembly** OwnerCap (e.g. `OwnerCap<StorageUnit>`, `OwnerCap<Gate>`) using [`access::transfer_owner_cap`](https://github.com/evefrontier/world-contracts/blob/main/contracts/world/sources/access/access_control.move) or [`transfer_owner_cap_with_receipt`](https://github.com/evefrontier/world-contracts/blob/main/contracts/world/sources/access/access_control.move#L126) if you borrowed it. The Character’s **OwnerCap** (i.e. `OwnerCap<Character>`) **cannot** be transferred to an address; only assembly caps can.

**If transferred to another address:** expect **limited EVE Frontier world functionality**. The Character’s `character_address` is not updated; many assembly functions require the transaction sender to equal `character.character_address()`. So a plain address holding the cap cannot perform those operations unless the **original character_address account** is the sender.

**Example:** In [`withdraw_by_owner`](https://github.com/evefrontier/world-contracts/blob/main/contracts/world/sources/assemblies/storage_unit.move) (storage unit):

```move
assert!(character.character_address() == ctx.sender(), ESenderCannotAccessCharacter);
```

*For this function, the caller must be the original character_address account.*

**For full EVE Frontier world functionality:** transfer the OwnerCap to a **managed shared object** that has the ability to borrow OwnerCaps back to the original designated `character_address` account. We recommend using the **hot-potato receipt pattern** for safety — the same pattern used for borrow/return in [`access_control.move`](https://github.com/evefrontier/world-contracts/blob/main/contracts/world/sources/access/access_control.move) (e.g. `ReturnOwnerCapReceipt`). That way, an account with the original character_address builds a transaction, borrows the cap from the shared object, performs the action (as sender), and must return or transfer the cap in the same transaction; the receipt prevents the cap from being dropped or lost.

**Reference:**
- [Ownership model — Transferring OwnerCap](../smart-contracts/ownership-model.md#transferring-ownercap)
- [`access_control.move`](https://github.com/evefrontier/world-contracts/blob/main/contracts/world/sources/access/access_control.move)
- [`character.move`](https://github.com/evefrontier/world-contracts/blob/main/contracts/world/sources/character/character.move)
