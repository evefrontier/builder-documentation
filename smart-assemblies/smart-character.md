# Smart Character

A Smart Character is the on-chain representation of an in-game character. It serves as the player's **identity on-chain** and the **owner of all assemblies** they create.

## Overview

When a player creates a character in-game, a corresponding `Character` object is created on-chain with the same character ID. The character is associated with a **tribe** and mapped to the player's **wallet address**.

## Character as Capability Holder

The character object acts as a **keychain** — it holds the `OwnerCap` for every object the player owns (network nodes, gates, storage units, etc.). See [Ownership Model](../smart-contracts/ownership-model.md) for details on the borrow-use-return pattern.

## Creation

Characters are created by the game server (admin) with a deterministic object ID derived from the in-game character ID.

## Discovering character from wallet address

The character object is a shared object; you need its object ID to interact with it. To get a character from a **wallet address** (e.g. when a player connects their wallet to your dApp), the game creates a **PlayerProfile** at character creation and [transfers it to the wallet](https://github.com/evefrontier/world-contracts/pull/119). It contains only `character_id`. Query objects owned by the wallet with type `PlayerProfile` to obtain the character ID, then fetch the `Character` object.

For a full GraphQL query and variables, see [Query character by wallet address](../tools/interfacing-with-the-eve-frontier-world.md#query-character-by-wallet-address).

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

**Reference:**
- [`character.move`](https://github.com/evefrontier/world-contracts/blob/main/contracts/world/sources/character/character.move)
